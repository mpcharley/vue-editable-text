<!--
MIT License

Copyright (c) 2020 mpcharley

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
-->
<template>
    <div>
        <a href="#" v-show="!editable" @click="onLinkClicked">{{ model }}</a>
        <span v-show="editable">
            <form @submit="onFormSubmit">
                <input class="input" ref="input" type="text" v-model="model" @blur="onInputBlur"/>
            </form>
        </span>
    </div>
</template>
<script>
    export default {
        name: 'EditableText',
        props: {
            value: String,
        },
        data(){
            return {
                editable: false,
            }
        },
        computed: {
            model: {
                get() { return (this.value.length === 0) ? '' : this.value },
                set(v) { this.$emit("input", v) }
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
                }
            },
            onLinkClicked(e) {
                e.preventDefault();
                this.editable = true
                this.$nextTick(() => this.$refs.input.focus())
            }
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