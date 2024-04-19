<script lang="ts">
    import {type Point, type Size} from '$lib/data/QwerkyMessages'
    import {type AppData, createAppData} from '$lib/data/AppData.svelte'
    import Panel from '$lib/panel/app_panel.svelte'
    import PageImage from '$lib/page/open_page.svelte'
    import img from '$lib/img.json'
    import inspects from '$lib/inspects.json'

    const appData: AppData = createAppData()
    let currentPageImage: { base64: string, size: Size } | null = $state(img)

    function onInspectPoint(event: CustomEvent<Point>) {
        appData.addInspectResult(inspects[0])
    }
</script>

{#if currentPageImage !== null}
    <PageImage boundingBoxes={appData.focusedInspection?.boundingBoxes}
               imageBase64={currentPageImage.base64}
               imageSize={currentPageImage.size}
               on:inspectPoint={onInspectPoint}/>
{/if}

<Panel appData={appData}/>

<!-- todo offsetting stacked highlights to not overlap requires extending highlight buffer by highlight count -->
<div class="hb t"></div>
<div class="hb l"></div>
<div class="hb b"></div>
<div class="hb r"></div>

<style>
    .hb {
        background: var(--panel-bg-color);
        position: fixed;
        z-index: var(--panel-bg-z-index);
    }

    .hb.t {
        height: var(--highlight-width);
        top: calc(var(--header-height) - var(--highlight-width));
        left: 0;
        right: 0;
    }

    .hb.l {
        top: var(--header-height);
        left: 0;
        bottom: var(--footer-height);
        width: var(--edge-width);
    }

    .hb.b {
        height: var(--highlight-width);
        bottom: calc(var(--footer-height) - var(--highlight-width));
        left: 0;
        right: 0;
    }

    .hb.r {
        top: var(--header-height);
        right: calc(var(--panel-width) - var(--highlight-width));
        bottom: var(--footer-height);
        width: var(--highlight-width);
    }
</style>
