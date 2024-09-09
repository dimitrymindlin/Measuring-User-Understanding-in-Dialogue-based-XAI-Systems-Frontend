<script lang="ts">
    import LikeButton from './LikeButton.svelte';
    import DislikeButton from './DislikeButton.svelte';
    import {createEventDispatcher} from 'svelte';
    import type {TChatMessage} from '$lib/types';
    import {fade} from 'svelte/transition';

    export let message: TChatMessage;

    const dispatch = createEventDispatcher();

    // Function to forward event
    function forwardEvent(event) {
        dispatch('feedbackButtonClick', event.detail);
    }

</script>

<div class="flex items-end mb-2.5 {message.isUser ? 'right-msg' : 'left-msg'}">
    <div class="msg-bubble max-w-md p-2.5 rounded-2xl">
        {@html message.text}
        {#if !message.isUser && message.feedback}
            <br/>
            <span class="float-right flex flex-wrap" in:fade={{delay: 1000, duration: 250}}>
                <LikeButton {message} on:feedbackButtonClick={forwardEvent}/>
                <DislikeButton {message} on:feedbackButtonClick={forwardEvent}/>
            </span>
        {/if}<br/>
    </div>
</div>

<style lang="postcss">
    .left-msg .msg-bubble {
        @apply rounded-bl-none variant-ghost-surface;
    }

    .right-msg .msg-bubble {
        @apply rounded-br-none variant-ghost-tertiary;
    }

    .follow-up-questions p {
        @apply font-semibold;
    }

    .msg-bubble .btn, .btn {
        @apply m-1 p-2 bg-transparent border border-solid border-gray-300 rounded cursor-pointer text-left block w-full;
        white-space: normal; /* Ensure text wraps */
        overflow: hidden; /* Keep the content within the button */
        max-width: 100%;
        transition: background-color 0.3s ease;

        &:hover {
            @apply bg-gray-200;
        }
    }
</style>
