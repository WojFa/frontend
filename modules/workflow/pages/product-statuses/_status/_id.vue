/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <ProductStatusPage
        :title="code"
        is-edit
        @dismiss="onDismiss"
        @remove="onRemove"
        @save="onSave" />
</template>

<script>

import { mapState, mapActions } from 'vuex';
import { getParentRoutePath } from '~/model/navigation/tabs';

export default {
    validate({ params }) {
        return /\w{8}-\w{4}-\w{4}-\w{4}-\w{12}/.test(params.id);
    },
    name: 'StatusEdit',
    components: {
        ProductStatusPage: () => import('~/components/Pages/ProductStatusPage'),
    },
    computed: {
        ...mapState('productStatus', {
            code: (state) => state.code,
            isDefaultStatus: (state) => state.isDefaultStatus,
        }),
    },
    methods: {
        ...mapActions('productStatus', [
            'updateProductStatus',
            'updateDefaultStatus',
            'removeProductStatus',
        ]),
        ...mapActions('validations', [
            'onError',
            'removeValidationErrors',
        ]),
        onSave() {
            const requests = [
                this.updateProductStatus({
                    onError: this.onError,
                }),
                this.updateDefaultStatus(),
            ];

            Promise.all(requests).then(() => {
                this.onProductStatusUpdated();
            });
        },
        onDismiss() {
            this.$router.push(getParentRoutePath(this.$route));
        },
        onRemove() {
            const isConfirm = confirm('Are you sure you want to delete this product status?'); /* eslint-disable-line no-restricted-globals */
            if (isConfirm) {
                this.removeProductStatus({
                    onSuccess: this.onRemoveProductStatusSuccess,
                });
            }
        },
        onRemoveProductStatusSuccess() {
            this.$addAlert({ type: 'success', message: 'Product status removed' });
            this.$router.push({ name: 'product-statuses' });
        },
        onProductStatusUpdated() {
            this.removeValidationErrors();
            this.$addAlert({ type: 'success', message: 'Product status updated' });
            this.$router.push({ name: 'product-statuses' });
        },
    },
    async fetch({
        store, params,
    }) {
        const path = `${store.state.authentication.user.language}/status/${params.id}`;

        await store.dispatch('productStatus/clearStorage');
        await store.dispatch('productStatus/getProductStatus', path);
        await store.dispatch('productStatus/getDefaultStatus');
    },
};
</script>
