<template>
    <div class="container">

        <!-- error when users variable is empty -->
        <div v-if="users.length === 0" class="box bg-yellow">
            <p>No users exist</p>
        </div>

        <!-- users loop -->
        <div v-if="users.length !== 0" class="box">
            <ul>
                <li v-for="user of users" :key="user.id">
                    <nuxt-link :to="{ name: 'users-id', params: { id: user.id, }, }">{{ user.username }}</nuxt-link>
                </li>
            </ul>
        </div>

        <!-- user create modal -->
        <modal @ok="storeUser" @cancel="closeCreateModal" v-show="modal.create" title="Create User">
            <form>
                <h3>Basic Data</h3>
                <input v-model="form.name" type="text" placeholder="Name" />
                <input v-model="form.username" type="text" placeholder="Username" />
                <input v-model="form.website" type="text" placeholder="Website" />
            </form>
        </modal>

        <!-- create user button -->
        <button @click="createUser" class="btn bg-blue txt-white">Create</button>

    </div>
</template>

<script>
    import Modal from '@/components/Modal';

    export default {


        components: {
            Modal,
        },


        mounted() {
            this.$store.dispatch('users/fetchUsers');
        },


        data() {
            return {
                modal: {
                    create: false,
                },

                form: {
                    name: '',
                    username: '',
                    website: '',
                },
            }
        },


        computed: {
            users() {
                return this.$store.getters['users/getUsers'];
            }
        },


        methods: {


            createUser() {
                this.openCreateModal();
            },



            openCreateModal() {
                this.resetForm();
                this.modal.create = true;
            },
            closeCreateModal() {
                this.modal.create = false;
                this.resetForm();
            },



            resetForm() {
                this.form = {};
            },


        },


    }
</script>
