<script lang="ts">
    import InferenceRule from "$lib/logic/InferenceRule.svelte";
    import {onMount} from "svelte";
    import {modusPonens, modusTollens} from "$lib/inferenceRules";
    import {SvelteMap} from "svelte/reactivity";

    onMount(() => {
        let script = document.createElement("script");
        script.src = "https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-svg.js";
        document.head.append(script);

        script.onload = () => {
            MathJax = {
                tex: {
                    inlineMath: [
                        ["$", "$"],
                        ["\\(", "\\)"],
                    ],
                },
                svg: {fontCache: "global"},
            };
        };
    });

    let map = new SvelteMap<string, string>();

    map.set("Modus Ponens", "If P, then Q. P. Therefore, Q.");
    map.set("Modus Tollens", "If P, then Q. It is not the case that Q. Therefore, It is not the case that P");
    let p = $state("");
    let q = $state("");
    let rule = $state("")

    let statement = $state("")

    let onValidArgCreation = () => {
        let templ = map.get(rule);
        if (!templ) {
            return;
        }

        statement = templ.replaceAll("P", p)
            .replaceAll("Q", q);
    }

</script>

<h1 class="text-4xl text-center">Valid Syllogism Creator</h1>
<div class="flex flex-row border border-neutral-500">

    <div class="flex flex-col basis-1/2">
        <div class="gap-10 flex flex-col items-start p-4 bg-neutral-800">
            <div class="flex w-full">
                <label for="p-premise">$$P\ $$</label>
                <input class="rounded px-2 bg-stone-900 w-full" name="p-premise"
                       bind:value={p}/><br/>
            </div>
            <div class="flex w-full">
                <label for="q-premise">$$Q\ $$</label>
                <input class="rounded px-2 bg-stone-900 w-full" name="q-premise"
                       bind:value={q}/><br/>
            </div>
            <select class="bg-stone-900 p-2 w-1/2 translate-x-5 rounded" bind:value={rule}>
                <option>Modus Ponens</option>
                <option>Modus Tollens</option>
            </select>
            <button onclick={() => onValidArgCreation()}
                    class="bg-stone-900 cursor-pointer rounded-full px-8 self-end py-2">
                Create
            </button>
        </div>

        <div class="rounded p-4 gap-2 flex flex-col bg-neutral-800">
            <div class="border-b">
                Inference Rule Cheatsheet
            </div>
            <div class="flex bg-stone-900">
                <InferenceRule name="Modus Ponens" source={modusPonens}></InferenceRule>
                <InferenceRule name="Modus Tollens" source={modusTollens}></InferenceRule>
            </div>
        </div>
    </div>


    <div class="basis-1/2 bg-neutral-950 p-10 flex flex-col justify-center">
        <div>

            <p class="text-4xl text-center">{statement}</p>
        </div>
    </div>

</div>
