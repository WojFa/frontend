/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <SegmentPage
        :title="code || 'New Segment'"
        @dismiss="onDismiss"
        @create="onCreate" />
</template>

<script>
import { mapState, mapActions } from 'vuex';
import { getParentRoutePath } from '~/model/navigation/tabs';

export default {
    name: 'SegmentNew',
    components: {
        SegmentPage: () => import('~/components/Pages/SegmentPage'),
    },
    computed: {
        ...mapState('segments', {
            code: (state) => state.code,
        }),
    },
    created() {
        this.clearStorage();
    },
    methods: {
        ...mapActions('segments', [
            'createSegment',
            'clearStorage',
        ]),
        ...mapActions('validations', [
            'onError',
            'removeValidationErrors',
        ]),
        onDismiss() {
            this.$router.push(getParentRoutePath(this.$route));
        },
        onCreateSegmentSuccess(id) {
            this.$addAlert({ type: 'success', message: 'Segment created' });
            this.$router.push({
                name: 'segment-edit-id-general',
                params: {
                    id,
                },
            });
        },
        onCreate() {
            const segment = {
                code: this.code,
            };

            this.removeValidationErrors();
            this.createSegment({
                data: segment,
                onSuccess: this.onCreateSegmentSuccess,
                onError: this.onError,
            });
        },
    },
};
</script>
