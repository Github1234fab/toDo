<script>
        import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";
        import { collection, addDoc } from "firebase/firestore";
        import db from "../../lib/firebase.js";

        let email = "";
        let password = "";

        const auth = getAuth();

        const signup = async (event) => {
                event.preventDefault();

                try {
                        const userCredential = await createUserWithEmailAndPassword(auth, email, password);
                        const user = userCredential.user;
                        console.log("User signed up successfully:", user);
                        // Ajouter l'utilisateur à la collection "users" dans Firestore
                        const usersCollection = collection(db, "users");
                        await addDoc(usersCollection, {
                                email: user.email,
                                // Ajoutez d'autres champs utilisateur ici si nécessaire
                        });
                } catch (error) {
                        const errorCode = error.code;
                        const errorMessage = error.message;
                        console.log("no sign");
                }
        };
</script>

<section class="auth-section">
        <form on:submit={signup}>
                <label for="E-mail">E-mail</label>
                <input type="email" bind:value={email} id="E-mail" name="E-mail" placeholder="Your e-mail" />
                <label for="Password">Password </label>
                <input
                        type="password"
                        bind:value={password}
                        id="Password"
                        name="Password"
                        placeholder="6 characters minimum characters"
                />

                <button type="submit">Sign Up</button>
                <p>
                        You haven't an account? <br />
                        Please, sign up or <a href="/Signin">sign in</a>
                </p>
        </form>
</section>
