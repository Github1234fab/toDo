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

<div class="app-section">
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
</div>

<style>
.app-section {
                display: flex;
                justify-content: center;
                align-items: center;
                min-height: 400px;
                flex-direction: column;
        } 
        ul {
                display: flex;
                list-style: none;
        }
        li {
                display: flex;
                justify-content: space-between;
                align-items: center;
                padding: 1rem;
                margin: 1rem;
                background-color: white;
                border-radius: 5px;
        
        }
        button {
                background-color: red;
                color: white;
                border: none;
                border-radius: 5px;
                padding: 3px;
                margin-left: 10px;
        }
        button:hover {
                background-color: darkred;
        } 
</style>
