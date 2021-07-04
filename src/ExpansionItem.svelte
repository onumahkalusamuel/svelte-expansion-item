<script>
    export let item = { heading: "", content: "" };
    export let group = "defaultGroup";
    export let expanded = false;

    import { groups } from "./store.js";
    import { slide } from "svelte/transition";
    import { onMount } from "svelte";

    let ID = function () {
   	return '_' + Math.random().toString(36).substr(2, 9);
    };

    onMount(function(){
	if($groups[group] === undefined) $groups[group] = "";
	if(expanded) $groups[group] = ID;
    });

    function updateExpanded () {
	if ($groups[group] === ID) {$groups[group] = ''}
	else {$groups[group] = ID}
    }

</script>

<style>
    .item {
        margin-top: 10px;
        margin-bottom: 10px;
    }
    .item-heading,
    .item-content {
        padding: 15px 5px 15px 15px;
        border: 1px solid #cccccc;
        text-align:left;
    }
    .item-content {
	padding-left:5px;
        border-top: none;
        line-height: 1.8;
    }
    .item-heading {
        cursor: pointer;
        font-weight: 600;
        font-size: 1.05em;
        display: flex;
        justify-content: space-between;
    }
    .item-heading:hover {
        background-color: rgba(0, 0, 0, 0.05);
    }
    span {
        font-size: 20px;
        font-weight: bold;
        padding: 5px;
    }
</style>

<div class="item">
    <div class="item-heading" on:click={updateExpanded}>
        <slot name="heading">
            {@html item.heading}
        </slot>
        <div>
            {#if $groups[group] === ID}
		<slot name="collapseIcon"><span>&minus;</span></slot>{:else}<slot name="expandIcon"><span>&plus;</span></slot>
	    {/if}
        </div>
    </div>
    {#if $groups[group] === ID}
        <div class="item-content" transition:slide>
            <slot name="content">
                {@html item.content}
            </slot>
        </div>
    {/if}
</div>
