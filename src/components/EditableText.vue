<template>
    <div>
        <a href="#" v-show="!editable" @click="onLinkClicked">{{ model }}</a>
        <span v-show="editable">
            <form @submit="onFormSubmit">
                <input class="input" ref="input" type="text" v-model="model" @blur="onInputBlur" @keyup="onKeyUp"/>
            </form>
        </span>
    </div>
</template>
<script>
    export default {
        name: 'EditableText',
        props: {
            value: String,
            defaultValue: String,
        },
        data() {
            return {
                editable: false,
            }
        },
        computed: {
            model: {
                get() {
                    let defaultValue;
                    if (undefined === this.defaultValue) {
                        defaultValue = 'n/a'
                    } else {
                        defaultValue = this.defaultValue
                    }
                    if(null === this.value) return defaultValue;
                    return this.value
                },
                set(v) {
                    this.$emit("input", v)
                }
            }
        },
        methods: {
            onFormSubmit: function (e) {
                e.preventDefault();
            },
            onInputBlur: function (e) {
                e.preventDefault();
                if (this.model.length > 0) {
                    this.$data.editable = false;
                    this.changed = false;
                } else {
                    let defaultValue;
                    if (undefined === this.defaultValue) {
                        defaultValue = 'n/a'
                    } else {
                        defaultValue = this.defaultValue
                    }
                    this.model = defaultValue;
                    this.$data.editable = false;
                    this.changed = false;
                }
            },
            onLinkClicked(e) {
                e.preventDefault();
                this.editable = true
                this.$nextTick(() => this.$refs.input.focus())
            },
            onKeyUp(e) {
                if (e.key === 'Enter') {
                    this.onInputBlur(e)
                }
            },
        },
        watch: {
            model: function (o, n) {
                this.changed = (o !== n);

            }
        },
    }
</script>

<style lang="scss" scoped>
    .input {

    }
</style>