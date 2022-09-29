<script>
  import "@carbon/styles/css/styles.css";
  import "carbon-components-svelte/css/all.css";
  import "@carbon/charts/styles.css";
  import { TextInput, Checkbox, Button, Form } from "carbon-components-svelte";
  import { writable } from "svelte/store";

  const formValue = {
    username: "",
  };
  const handleSubmit = (e) => {
    e.preventDefault();

    if (!formValue.username) {
      alert("Username is missing!");
      return;
    }
    console.log(formValue.username);
  };
</script>

<div>
  <h2>Login page</h2>

  <Form on:submit={handleSubmit}>
    <TextInput
      bind:value={formValue.username}
      id="username"
      invalidText="Username is required"
      placeholder="Enter Username"
    />

    <Button kind="primary" type="submit">Sign In</Button>
  </Form>

  <div>
    <span>not register yet?</span>
    <span>
      <a href="/Register">Registation</a>
    </span>
  </div>
</div>

<style>
</style>
