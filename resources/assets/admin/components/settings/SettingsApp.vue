<template>
    <div v-loading="!app_ready" class="settings_app">
        <router-view
            v-if="app_ready"
            :form_id="form_id"
            :form="form"
            :inputs="inputs"
            :has_pro="hasPro"
            :has_pdf="hasPDF"
            :editorShortcodes="editorShortcodes"
        ></router-view>
    </div>
</template>

<script type="text/babel">
    export default {
        name: 'settings_app',
        data() {
            return {
                app_ready: false,
                form_id: window.FluentFormApp.form_id,
                form: {
                    id: window.FluentFormApp.form_id,
                },
                hasPro: !!window.FluentFormApp.hasPro,
                hasPDF: !!window.FluentFormApp.hasPDF,
                editorShortcodes: [],
                inputs: {}
            }
        },
        methods: {
            fetchInputs() {
                let data = {
                    action: this.$action.getFormInputs,
                    formId: this.form_id
                };
                jQuery.get(ajaxurl, data)
                    .done(response => {
                        this.inputs = Object.assign({}, response);
                        this.app_ready = true;
                    })
                    .fail(e => {
                    });
            },
            fetchAllEditorShortcodes() {
                let data = {
                    action: this.$action.getAllEditorShortcodes,
                    formId: this.form_id,
                    input_only: true
                };
                jQuery.get(ajaxurl, data)
                    .done(response => {
                        let allShortCodes = response;
                        if (allShortCodes[0] && allShortCodes[0]['shortcodes']) {
                            delete allShortCodes[0]['shortcodes']['{all_data}'];
                        }

                        this.editorShortcodes = allShortCodes;
                        this.app_ready = true;
                    })
                    .fail(e => {
                    });
            },
        },
        mounted() {
            this.fetchInputs();
            this.fetchAllEditorShortcodes();

            let currentActive = jQuery('.ff_settings_list a[href="#' + this.$route.fullPath + '"]');

            if(currentActive.length) {
                currentActive.parent().addClass('active');
            } else {
                jQuery('.ff_settings_list li:first-child').addClass('active');
            }


            jQuery('.ff_settings_list a').on('click', function () {
                jQuery('.ff_settings_list li').removeClass('active');
                jQuery(this).parent().addClass('active');
            });

            jQuery('head title').text('Settings & Integrations - Fluent Forms');

        }
    }
</script>
