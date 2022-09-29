<script>
  import "@carbon/styles/css/styles.css";
  import "@carbon/charts/styles.css";
  import { preformatMakeCredReq, publicKeyCredentialToJSON } from "../helper";
  import { TextInput, Checkbox, Button, Form } from "carbon-components-svelte";

  const formValue = {
    username: "",
    name: "",
  };

  const handleSubmit = (e) => {
    e.preventDefault();

    if (!formValue.username || !formValue.name) {
      alert("Name or Username is missing!");
      return;
    }
    console.log(formValue.username, formValue.name);
    let username = formValue.username;
    let name = formValue.name;

    getMakeCredentialsChallenge({ username, name })
      .then((response) => {
        let publicKey = preformatMakeCredReq(response);
        return navigator.credentials.create({ publicKey });
      })
      .then((newCred) => {
        console.log(newCred);
        let makeCredResponse = publicKeyCredentialToJSON(newCred);
        console.log(makeCredResponse);
        return sendWebAuthnResponse(makeCredResponse);
      })

      .then((response) => {
        if (response.status === "ok") {
          // loadMainContainer();
          window.location.replace("/mainbhhd");
        } else {
          alert(
            `Server responed with error. The message is: ${response.message}`
          );
        }
      })
      .catch((error) => alert(error));
  };

  let getMakeCredentialsChallenge = (formBody) => {
    return fetch("http://localhost:3000/webauthn/register", {
      method: "POST",
      credentials: "include",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(formBody),
    })
      .then((response) => response.json())
      .then((response) => {
        if (response.status !== "ok")
          throw new Error(
            `Server responed with error. The message is: ${response.message}`
          );

        return response;
      });
  };

  let sendWebAuthnResponse = (body) => {
    return fetch("http://localhost:3000/webauthn/response", {
      method: "POST",
      credentials: "include",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(body),
    })
      .then((response) => response.json())
      .then((response) => {
        if (response.status !== "ok")
          throw new Error(
            `Server responed with error. The message is: ${response.message}`
          );

        return response;
      });
  };
</script>

<div>
  <h2>Register page</h2>
  <Form on:submit={handleSubmit}>
    <TextInput
      bind:value={formValue.name}
      id="name"
      invalidText="Name is required"
      labelText="Name"
      placeholder="Enter Name"
    />
    <TextInput
      bind:value={formValue.username}
      id="username"
      invalidText="Username is required"
      labelText="User name"
      placeholder="Enter Username"
    />
    <Button type="submit">Register</Button>
  </Form>
  <div>
    <span>Already register?</span>
    <span><a href="/">Login Page</a></span>
  </div>
</div>

<style>
</style>
