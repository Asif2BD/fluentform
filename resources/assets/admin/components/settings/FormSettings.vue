<template>
    <div v-loading="!formSettings" element-loading-text="Loading Settings..." style="min-height: 100px;">
        <template v-if="formSettings">
            <!--Save settings-->
            <!-- Confirmation Settings -->
            <el-row class="setting_header">
                <el-col :md="12">
                    <h2>Confirmation Settings</h2>
                </el-col>

                <!--Save settings-->
                <el-col :md="12" class="action-buttons clearfix mb15">
                    <el-button
                            :loading="loading"
                            class="pull-right"
                            size="medium"
                            type="success"
                            icon="el-icon-success"
                            @click="saveSettings">
                        {{loading ? 'Saving' : 'Save'}} Settings
                    </el-button>
                </el-col>
            </el-row>

            <!--confirmation settings form-->
            <el-form label-width="205px" label-position="left">
                <add-confirmation
                        :pages="pages"
                        :editorShortcodes="editorShortcodes"
                        :confirmation="formSettings.confirmation"
                        :errors="errors">
                </add-confirmation>
            </el-form>

            <!-- Appearance Settings -->
            <el-row class="setting_header">
                <el-col :md="24">
                    <h2>Form Layout</h2>
                </el-col>
            </el-row>

            <!--Appearance settings form-->
            <el-form label-width="205px" label-position="left">
                <!--Label placement-->
                <el-form-item>
                    <template slot="label">
                        Label Alignment

                        <el-tooltip class="item" placement="bottom-start" effect="light">
                            <div slot="content">
                                <h3>Form Label Placement</h3>

                                <p>
                                    Select the default label placement. Labels can be <br>
                                    top aligned above a field, left aligned to the <br>
                                    left of a field, or right aligned to the right of a field.
                                </p>
                            </div>

                            <i class="el-icon-info el-text-info"></i>
                        </el-tooltip>
                    </template>

                    <el-radio v-for="(labelOption, optionName) in labelPlacementOptions"
                              v-model="formSettings.layout.labelPlacement" :label="optionName"
                              :key="optionName" border>
                        {{ labelOption }}
                    </el-radio>
                </el-form-item>

                <!--Help Message placement-->
                <el-form-item>
                    <template slot="label">
                        Help Message Position

                        <el-tooltip class="item" placement="bottom-start" effect="light">
                            <div slot="content">
                                <h3>Help Message Placement</h3>

                                <p>
                                    Select the default help message placement. <br>
                                    Help messages can be placed beside <br>
                                    label as a tooltip, or bellow each input.
                                </p>
                            </div>

                            <i class="el-icon-info el-text-info"></i>
                        </el-tooltip>
                    </template>

                    <el-radio v-for="(option, optionName) in helpMessagePlacementOptions"
                              v-model="formSettings.layout.helpMessagePlacement" :label="optionName"
                              :key="optionName" border> {{ option }}
                    </el-radio>
                </el-form-item>

                <!--Error Message placement-->
                <el-form-item>
                    <template slot="label">
                        Error Message Position

                        <el-tooltip class="item" placement="bottom-start" effect="light">
                            <div slot="content">
                                <h3>Error Message placement</h3>

                                <p>
                                    Select the default error message placement. <br>
                                    Error messages can be placed bellow each input, <br>
                                    or stacked after the form submit button.
                                </p>
                            </div>

                            <i class="el-icon-info el-text-info"></i>
                        </el-tooltip>
                    </template>

                    <el-radio v-for="(option, optionName) in errorMessagesPlacement"
                              v-model="formSettings.layout.errorMessagePlacement" :label="optionName"
                              :key="optionName" border>{{ option }}
                    </el-radio>
                </el-form-item>

                <!--Required asterisk mark position -->
                <el-form-item>
                    <template slot="label">
                        Asterisk Position

                        <el-tooltip class="item" placement="bottom-start" effect="light">
                            <div slot="content">
                                <h3>Required Asterisk Position</h3>

                                <p>
                                    The asterisk marker position for the required elements
                                </p>
                            </div>

                            <i class="el-icon-info el-text-info"></i>
                        </el-tooltip>
                    </template>

                    <el-radio v-for="(option, optionName) in asteriskPlacementMock"
                              v-model="formSettings.layout.asteriskPlacement" :label="optionName"
                              :key="optionName" border>{{ option }}
                    </el-radio>
                </el-form-item>
            </el-form>

            <!-- Form Restrictions -->
            <el-row class="setting_header">
                <el-col :md="24">
                    <h2>Scheduling & Restrictions</h2>
                </el-col>
            </el-row>

            <!--Restriction settings form-->
            <div class="ff_settings_section">
                <div class="ff_settings_body">
                    <form_restriction :data="formSettings.restrictions"></form_restriction>
                </div>
            </div>

            <div class="ff_advanced_validation_wrapper">
                <!-- Header -->
                <el-row class="setting_header">
                    <el-col :md="24">
                        <h2>Advanced Form Validation</h2>
                        <p>
                            You can set rules to the user input and based on the rules you can prevent the form submit.
                            This is very useful feature for preventing spam/bot submissions.
                        </p>
                    </el-col>
                </el-row>
                <!-- Form Body -->
                <div class="ff_settings_section">
                    <div class="ff_settings_body">
                        <advanced-validation :inputs="inputs"
                                             :settings="advancedValidationSettings"></advanced-validation>
                    </div>
                </div>
            </div>


            <!-- Survey Result -->
            <el-row class="setting_header">
                <el-col :md="24">
                    <h2>Survey Result</h2>
                </el-col>
            </el-row>

            <div class="ff_settings_section">
                <div class="ff_settings_body">
                    <survey-result :data="formSettings.appendSurveyResult" :hasPro="hasPro"/>

                    <p v-if="!hasPro"><br/>This feature is only available in pro version of WP Fluent Forms</p>
                </div>
            </div>

            <el-row class="setting_header">
                <el-col :md="24">
                    <h2>
                        Compliance Settings
                        <el-tooltip class="item" placement="bottom-start" effect="light">
                            <div slot="content">
                                <h3>Delete entry on form submission</h3>

                                <p>
                                    if you enable this settings then your entry data will be deleted from database. It's
                                    useful for HIPPA/GDPR Compliance for some forms
                                </p>
                            </div>

                            <i class="el-icon-info el-text-info"></i>
                        </el-tooltip>
                    </h2>
                </el-col>
            </el-row>
            <div class="ff_settings_section">
                <div class="ff_settings_body">
                    <el-checkbox :disabled="!hasPro" true-label="yes" false-label="no"
                                 v-model="formSettings.delete_entry_on_submission">Delete entry data after form
                        submission
                    </el-checkbox>

                    <p v-if="!hasPro"><br/>This feature is only available in pro version of WP Fluent Forms</p>

                    <p v-if="formSettings.delete_entry_on_submission == 'yes'"><br/>Your data will be deleted on form
                        submission so no entry data, analytics and visual reporting will be available for this form </p>
                </div>
            </div>

            <br/><br/>
            <el-row class="setting_header">
                <el-col :md="24">
                    <h2>
                        Other
                    </h2>
                </el-col>
            </el-row>

            <div class="ff_settings_section">
                <div class="ff_settings_body">
                    <div class="el-form-item">
                        <label class="el-form-item__label" style="width: 205px; text-align: left;">
                            Extra CSS Form Class
                        </label>
                        <div class="el-form-item__content" style="margin-left: 205px;">
                            <el-input
                                    :disabled="!hasPro"
                                    placeholder="extra css class"
                                    size="small"
                                    v-model="formSettings.form_extra_css_class"/>
                        </div>
                    </div>
                    <p v-if="!hasPro"><br/>This feature is only available in pro version of WP Fluent Forms</p>
                </div>
            </div>

            <el-row style="margin-top: 50px">
                <el-button
                        :loading="loading"
                        class="pull-right"
                        size="medium"
                        type="success"
                        icon="el-icon-success"
                        @click="saveSettings">
                    {{loading ? 'Saving' : 'Save'}} Settings
                </el-button>
            </el-row>
        </template>
    </div>
</template>

<script type="text/babel">
    import wpEditor from '../../../common/_wp_editor';
    import form_restriction from './FormSettings/Restrictions';
    import SurveyResult from './FormSettings/SurveyResult';
    import errorView from '../../../common/errorView';
    import AddConfirmation from './Includes/AddConfirmation.vue'
    import AdvancedValidation from "./Includes/AdvancedValidation";

    export default {
        name: 'FormSettings',
        props: {
            'form': Object,
            'form_id': [Number, String],
            'editor-shortcodes': {
                type: Array,
                default: []
            },
            'inputs': Object
        },
        components: {
            wpEditor,
            'form_restriction': form_restriction,
            errorView,
            AddConfirmation,
            SurveyResult,
            AdvancedValidation
        },
        data() {
            return {
                savingSettings: false,
                loading: false,
                redirectToOptions: {
                    samePage: 'Same Page',
                    customPage: 'To a Page',
                    customUrl: 'To a Custom URL'
                },
                labelPlacementOptions: {
                    'top': 'Top',
                    'left': 'Left',
                    'right': 'Right'
                },
                helpMessagePlacementOptions: {
                    'with_label': 'Beside Label (Tooltip)',
                    'under_input': 'Below Input Fields',
                    'on_focus': 'Focus on Element'
                },
                errorMessagesPlacement: {
                    'inline': 'Below Input Fields',
                    'stackToBottom': 'Stacked after Form'
                },
                asteriskPlacementMock: {
                    '': 'None',
                    'asterisk-left': 'Left to Label',
                    'asterisk-right': 'Right to Label'
                },
                advancedValidationSettings: {
                    status: false,
                    type: 'all',
                    conditions: [
                        {
                            field: null,
                            operator: '=',
                            value: null
                        }
                    ],
                    error_message: ''
                },
                pages: [],
                formSettings: false,
                isPageLoading: true,
                errors: new Errors,
                delete_entry_on_submission: 'no',
                hasPro: !!window.FluentFormApp.hasPro
            }
        },
        methods: {
            setDefaultSettings() {
                this.formSettings = {
                    confirmation: {
                        redirectTo: 'samePage',
                        messageToShow: 'Thank you for your message. We will get in touch with you shortly',
                        customPage: null,
                        samePageFormBehavior: 'hide_form',
                        customUrl: null,
                    },
                    restrictions: {
                        limitNumberOfEntries: {
                            enabled: false,
                            numberOfEntries: null,
                            period: 'total',
                            limitReachedMsg: 'Maximum number of entries exceeded.',
                        },
                        scheduleForm: {
                            enabled: false,
                            start: null,
                            end: null,
                            pendingMsg: "Form submission is not started yet.",
                            expiredMsg: "Form submission is now closed.",
                        },
                        requireLogin: {
                            enabled: false,
                            requireLoginMsg: 'You must be logged in to submit the form.',
                        },
                        denyEmptySubmission: {
                            enabled: false,
                            message: 'Sorry, you cannot submit an empty form. Let\'s hear what you wanna say.'
                        }
                    },
                    layout: {
                        labelPlacement: 'top',
                        asteriskPlacement: 'asterisk-right',
                        helpMessagePlacement: 'with_label',
                        errorMessagePlacement: 'inline',
                        cssClassName: ''
                    }
                }
            },
            fetchSettings() {
                this.$ajax.get('getFormGeneralSettings', {form_id: this.form_id})
                    .done(response => {
                        if (response.data.generalSettings) {
                            let settings = response.data.generalSettings;
                            if (!settings.confirmation)
                                settings.confirmation = {};
                            if (!settings.restrictions)
                                settings.restrictions = {};
                            if (!settings.layout)
                                settings.layout = {};

                            if (!settings.restrictions.limitNumberOfEntries)
                                settings.restrictions.limitNumberOfEntries = {};
                            if (!settings.restrictions.scheduleForm)
                                settings.restrictions.scheduleForm = {};
                            if (!settings.restrictions.requireLogin)
                                settings.restrictions.requireLogin = {};

                            if (!settings.restrictions.denyEmptySubmission)
                                settings.restrictions.denyEmptySubmission = {};

                            if (!settings.appendSurveyResult) {
                                settings.appendSurveyResult = {
                                    enabled: false,
                                    showLabel: false,
                                    showCount: false
                                }
                            }
                            this.formSettings = settings;
                        } else {
                            this.setDefaultSettings();
                        }
                        this.advancedValidationSettings = response.data.advancedValidationSettings;
                    })
                    .fail(e => {
                        this.setDefaultSettings();
                    })
                    .always(() => {
                    });
            },
            fetchPages() {
                this.$ajax.get('getPages', {form_id: this.form_id})
                    .success(response => {
                        this.pages = response.data.pages;
                    })
                    .fail(e => {
                        this.loading = false;
                    })
            },
            saveSettings() {
                this.loading = true;
                let data = {
                    form_id: this.form_id,
                    formSettings: JSON.stringify(this.formSettings),
                    advancedValidationSettings: JSON.stringify(this.advancedValidationSettings)
                };

                this.$ajax.post('saveFormGeneralSettings', data)
                    .then(response => {
                        this.$notify.success({
                            title: 'Success',
                            message: response.data.message,
                            offset: 30
                        });
                    })
                    .fail(error => {
                        this.errors.record(error.responseJSON.data.errors);
                    })
                    .always(() => {
                        this.loading = false;
                    });
            }
        },
        mounted() {
            this.fetchSettings();
            this.fetchPages();
            jQuery('head title').text('Confirmation Settings - Fluent Forms');
        },
        beforeCreate() {
            ffSettingsEvents.$emit('change-title', 'Form Settings');
        }
    };
</script>

<style lang="scss">
    @import "../../styles/el_customize";

    .el-form-item__error {
        position: relative !important;

        p {
            margin-bottom: 0px;
            margin-top: 0px;
        }
    }
</style>