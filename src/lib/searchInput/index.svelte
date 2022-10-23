<script>
  import "./style.css";
  import { Form, Input, Button, InputGroup } from "sveltestrap";

  export let query, handleSubmit, handleChange;
</script>

<main>
  <Form on:submit={handleSubmit}>
    <InputGroup>
      <Input
        class="form-control bg-transparent text-light formSearchBox"
        type="text"
        placeholder="Search city's weather here"
        value={query}
        on:change={handleChange}
      />
      <Button class="text-light formButton" type="submit">Search</Button>
    </InputGroup>
  </Form>
</main>
