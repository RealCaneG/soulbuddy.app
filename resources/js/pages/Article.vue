<template>
    <page-template>
        <div slot="bannerHeader" class="banner"/>
        <p slot="bannerDescription"></p>
        <div slot="tools" class="tool-container">
            <div class="category-container">
                <b-form-select :options="this.categories" v-model="category"></b-form-select>
            </div>
            <div class="wrapper-center">
                <b-button class="button" v-b-modal.create-article-modal @click="$bvModal.show('create-article-modal')">
                    Write an Article
                </b-button>
            </div>
            <b-modal class="dialog" busy="true" centered id="create-article-modal">
                <template v-slot:modal-title>
                    <h2>Write an Article</h2>
                </template>
                <create-article-component
                    v-on:onSubmit="submitForm"></create-article-component>
<!--                <template v-slot:modal-footer>-->
<!--                    <p></p>-->
<!--                </template>-->
            </b-modal>
        </div>
        <template>
            <all-articles-component slot="content" :category="category"></all-articles-component>
        </template>
    </page-template>
</template>

<script>
    import PageTemplate from "../components/pageTemplate";

    export default {
        name: "Article",
        components: {PageTemplate},
        data() {
            return {
                categories: this.$store.state.categories,
                category: null,
            }
        },
        mounted() {
            if (this.$store.state.categories.length <= 1) {
                this.$store.dispatch('getCategories')
            }
        },

        methods: {
            openDialog() {

            },

            submitForm(formData) {
                axios
                    .post("/article/create_article", formData, {
                        headers: {"Content-Type": "multipart/form-data"}
                    })
                    .then(res => {
                        this.$bvModal.hide('create-article-modal');
                        this.$store.dispatch('refreshArticles', 1)
                    });
            },
        }
    }
</script>

<style scoped>
    .dialog {
        margin-top: 5rem;
    }

    .wrapper-center {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .banner {
        background-image: url("/images/article-banner.jpg");
        background-repeat: no-repeat;
        background-size: 100%;
        background-position: center;
        height: 30%;
        overflow: hidden;
    }

    .tool-container {
        text-align: center;
    }

    .tools-desc {
        text-align: right;
        font-family: 'Arvo', serif;
        font-weight: 600;
        font-size: .9rem;
        margin-right: 1rem;
    }

    button {
        text-align: center;
        font-size: 1rem;
        color: white;
        padding: 0.5rem 3rem;
        border: 1px solid #F63854;
        border-radius: 2rem;
        background-color: #f63854;
    }

    button:hover {
        color: white;
        background-color: #F63854;
    }

    button:focus {
        outline: none;
    }

    .category-container {
        display: flex;
        justify-content: flex-end;
        align-items: center;
    }
</style>
