<script>
        import { addDoc, collection, onSnapshot, deleteDoc } from "firebase/firestore";
        import db from "../lib/firebase.js";
        import { onMount } from "svelte";
        //pour récuperer la value de l'input.
        let produit = "";
        let snap;

        //instantané de la base de donnée
        onMount(() => {
                onSnapshot(collection(db, "todos"), (snapshot) => {
                        snap = snapshot.docs;
        });
        });

        //function async pour ajouter un produit dans la base de donnée.
        async function handleSubmit(event) {
                event.preventDefault();
                console.log(db);

                const inputData = {
                        produit: produit.trim(),
                };

                await addDoc(collection(db, "todos"), inputData);
        };

        //function async pour supprimer un produit dans la base de donnée.
        // async function handleClick(event) {
        //         await deleteDoc(collection(db, "todos"), event.target.id);
        //         console.log("Document successfully deleted!");
        // };

</script>

<form on:submit={handleSubmit}>
        <input type="text" bind:value={produit} />
        <button type="submit">Submit</button>
</form>
{#if snap}
        <ul>
                {#each snap as item }
                        <li>
                                {item.data().produit}
                                <button >x</button>
                        </li>
                {/each}
        </ul>
{/if}
