/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <UserPage
        title="New User"
        @dismiss="onDismiss"
        @create="onCreate" />
</template>

<script>

import { mapState, mapActions } from 'vuex';
import { getParentRoutePath } from '~/model/navigation/tabs';

export default {
    name: 'NewUser',
    components: {
        UserPage: () => import('~/components/Pages/UserPage'),
    },
    computed: {
        ...mapState('users', {
            email: (state) => state.email,
            firstName: (state) => state.firstName,
            lastName: (state) => state.lastName,
            language: (state) => state.language,
            password: (state) => state.password,
            passwordRepeat: (state) => state.passwordRepeat,
            isActive: (state) => state.isActive,
            roleId: (state) => state.roleId,
        }),
    },
    created() {
        this.clearStorage();
    },
    methods: {
        ...mapActions('users', [
            'createUser',
            'clearStorage',
        ]),
        ...mapActions('validations', [
            'onError',
            'removeValidationErrors',
        ]),
        onDismiss() {
            this.$router.push(getParentRoutePath(this.$route));
        },
        onCreateUserSuccess(id) {
            this.removeValidationErrors();
            this.$addAlert({ type: 'success', message: 'User created' });
            this.$router.push({
                name: 'user-edit-id',
                params: {
                    id,
                },
            });
        },
        onCreate() {
            const user = {
                email: this.email,
                firstName: this.firstName,
                lastName: this.lastName,
                password: this.password,
                passwordRepeat: this.passwordRepeat,
                language: this.language,
                roleId: this.roleId,
                isActive: this.isActive,
            };
            this.createUser({
                data: user,
                onSuccess: this.onCreateUserSuccess,
                onError: this.onError,
            });
        },
    },
    async fetch({
        store,
    }) {
        await store.dispatch('roles/getRoles', {
            limit: 9999,
            offset: 0,
        });
    },
};
</script>