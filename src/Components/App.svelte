<script>
        import { addDoc, collection, onSnapshot, deleteDoc, doc } from "firebase/firestore";
        import db from "../lib/firebase.js";
        import { onMount } from "svelte";
        let produit = "";
        let snap;
        let inputData;

        onMount(() => {
                onSnapshot(collection(db, "todos"), (snapshot) => {
                        snap = snapshot.docs;
                });
        });

        async function handleSubmit(event) {
                event.preventDefault();
                console.log(db);

                inputData = {
                        produit: produit.trim(),
                        id: crypto.randomUUID(),
                };

                await addDoc(collection(db, "todos"), inputData);
        }

        async function handleClick(id) {
                const docRef = doc(db, "todos", id);
                await deleteDoc(docRef);
                console.log("Document successfully deleted!");
        }
</script>

<form on:submit={handleSubmit}>
        <input type="text" bind:value={produit} />
        <button type="submit">Submit</button>
</form>
{#if snap}
        <ul>
                 {#each snap as doc}
                        <li>
                                {doc.data().produit}
                                <button on:click={() => handleClick(doc.id)}>x</button>
                        </li>
                {/each} 
        </ul>
{/if}