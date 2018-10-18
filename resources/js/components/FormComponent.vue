<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card card-default">
                    <div class="card-header">Example Component</div>

                    <div class="card-body">
                        <textarea v-model="value" />
                        <span>Символов введено: {{charCount}}</span>
                        <button @click="transliterate">Транслитерировать</button>
                        <button @click="untransliterate">Растранслитерировать</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        data: () => ({
            value: '',
            }),
        computed: {
            charCount: function () {return this.value.length;},
        },
        methods: {
            transliterate: function(direction) {
                let regexp = null;
                let newText = text;
                TranslitTable.forEach(char => {
                    regexp = new RegExp(char.ru, 'g');
                    newText = newText.replace(regexp, char.lat);
                });
                this.value = newText;
            },
        untransliterate: function () {
                let regexp = null;
                let newText = text;
                TranslitTable.forEach(char => {
                    regexp = new RegExp(char.lat, 'g');
                    newText = newText.replace(regexp, char.ru);
                });
                this.value = newText;
            }
        }
    }
</script>
