<script lang="ts">
    import type {Rect} from '$lib/data/QwerkyMessages'

    interface BoundingBoxProps {
        color: string
        index: number
        rect: Rect
    }

    let {color, index, rect}: BoundingBoxProps = $props()

    let hover: boolean = $state(false)
</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->

<div class="highlight-{index}"
     class:hover
     style="--highlight-color: {color}; --element-x: {rect.x}; --element-y: {rect.y}; --element-w: {rect.w}; --element-h: {rect.h};"
     onfocus={() => hover = true}
     onblur={() => hover = false}
     onmouseover={() => {hover = true; return true}}
     onmouseout={() => {hover = false; return true}}>
</div>

<style>
    div {
        pointer-events: none;
        z-index: var(--page-highlight-z-index);
        position: absolute;
        border: var(--highlight-width) solid var(--highlight-color);
        top: calc(var(--page-scroll-y) + (var(--page-scale-ar) * var(--element-y)) - var(--highlight-width));
        left: calc(var(--page-scale-ar) * var(--element-x) - var(--highlight-width));
        width: calc(var(--page-scale-ar) * var(--element-w));
        aspect-ratio: var(--element-w, 1) / var(--element-h, 1);
    }
</style>
