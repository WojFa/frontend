/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <ResponsiveCenteredViewTemplate :fixed="true">
        <template #content>
            <VerticalFixedScroll>
                <SegmentsForm />
            </VerticalFixedScroll>
        </template>
    </ResponsiveCenteredViewTemplate>
</template>

<script>
import SegmentsForm from '~/components/Forms/SegmentsForm';
import ResponsiveCenteredViewTemplate from '~/core/components/Layout/Templates/ResponsiveCenteredViewTemplate';
import VerticalFixedScroll from '~/core/components/Layout/Scroll/VerticalFixedScroll';

export default {
    name: 'SegmentBaseTab',
    components: {
        VerticalFixedScroll,
        SegmentsForm,
        ResponsiveCenteredViewTemplate,
    },
};
</script>
