<template>
    <div id="app">
        <header>
            <h1><span>To-Do</span></h1>
        </header>
        <section>
            <aside>
                <Categories v-bind:categories="categories"
                            v-model="selectedId"
                />
            </aside>
            <main>
                <Tasks v-bind:selectedId="selectedId"/>
            </main>
        </section>
    </div>
</template>

<script>
    import Categories from './components/Categories.vue'
    import Tasks from './components/Tasks.vue'

    export default {
        name: 'app',
        props: {
            categories: Array,
            tasks: Array,
        },
        data() {
            return {
                selectedId: '',
            }
        },
        components: {
            Categories,
            Tasks,
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        width: 90%;
        margin: 0 auto;
    }

    #app > section {
        display: flex;
    }

    #app > section > aside {
        flex: 1;
        padding-right: 20px;
    }

    #app > section > main {
        flex: 2;
    }

    header {
    }

    header h1:before,
    header h1:after {
        display: inline-block;
        vertical-align: middle;
        content: '';
        width: 26px;
        height: 26px;
        background: url(./assets/logo.png) no-repeat;
        background-size: 26px 26px;
    }

    header h1:before {
        transform: rotate(100grad);
        margin-right: 6px;
    }

    header h1:after {
        transform: rotate(-100grad);
        margin-left: 6px;
    }

    header span {
        display: inline-block;
        vertical-align: middle;
    }
</style>
