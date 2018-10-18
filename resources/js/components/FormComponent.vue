<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card card-default">
                    <div class="card-header">Example Component</div>

                    <div class="card-body">
                        <textarea v-model="value" />
                        <span>Символов введено: {{charCount}}</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        // methods: {
        //     handleChange: function() {

        //     }
        // },
        data: () => ({
            value: '',
            }),
        computed: {
            charCount: function () {return this.value.length;}
        }
    }
</script>
