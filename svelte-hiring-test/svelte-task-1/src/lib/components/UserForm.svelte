<script lang="ts">
  // TODO: Implement form state management
  // TODO: Implement form validation
  // TODO: Implement submit handler
  // TODO: Implement success state management
  import { writable } from "svelte/store";
  //   import type { UserFormData, FormErrors } from "App";

  const formData = writable<App.UserFormData>({
    firstName: "",
    lastName: "",
    email: "",
    password: "",
  });
  const formErrors = writable<App.FormErrors>({});

  //   const errors = writable<App.FormErrors>({});
  const successMessage = writable<string | null>(null);

  function validateForm(data: App.UserFormData): App.FormErrors {
    const error: App.FormErrors = {};

    if (!data.firstName.trim()) error.firstName = "First name is required";
    if (!data.lastName.trim()) error.lastName = "Last name is required";
    if (!data.email.includes("@")) error.email = "Email must be valid";
    if (data.password.length < 6)
      error.password = "Password must atleast be 6 characters";
    return error;
  }

  function handleSubmit(event: Event) {
    event.preventDefault();
    formData.update((data) => {
      const error = validateForm(data);
      formErrors.set(error);

      if (Object.keys(error).length === 0) {
        successMessage.set("Form submitted successfully");
      } else {
        successMessage.set(null);
      }
      return { ...data };
    });
  }
</script>

<div class="form-container">
  <form on:submit={handleSubmit}>
    <div class="form-group">
      <label for="firstName">First Name</label>
      <input
        id="firstName"
        type="text"
        placeholder="Enter your first name"
        bind:value={$formData.firstName}
        class:input-error={$formErrors.firstName}
      />

      {#if $formErrors.firstName}
        <span class="error-message">{$formErrors.firstName}</span>
      {/if}
    </div>

    <div class="form-group">
      <label for="lastName">Last Name</label>
      <input
        id="lastName"
        type="text"
        placeholder="Enter your last name"
        bind:value={$formData.lastName}
      />
      {#if $formErrors.lastName}
        <span class="error-message">{$formErrors.lastName}</span>
      {/if}
    </div>

    <div class="form-group">
      <label for="email">Email</label>
      <input
        id="email"
        type="email"
        placeholder="Enter your email"
        bind:value={$formData.email}
      />
      {#if $formErrors.email}
        <span class="error-message">{$formErrors.email}</span>
      {/if}
    </div>

    <div class="form-group">
      <label for="password">Password</label>
      <input
        id="password"
        type="password"
        placeholder="Enter your password"
        bind:value={$formData.password}
      />
      {#if $formErrors.password}
        <span class="error-message">{$formErrors.password}</span>
      {/if}
    </div>

    <button type="submit" class="submit-button">Submit</button>
  </form>
  {#if $successMessage}
    <div class="success-message">{$successMessage}</div>
  {/if}
  <!-- TODO: Add success message section here -->
</div>

<style>
  .form-container {
    max-width: 480px;
    margin: 0 auto;
    padding: 2rem;
    background-color: white;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .form-group {
    margin-bottom: 1.5rem;
  }

  label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: 500;
    color: #374151;
  }

  input {
    width: 100%;
    padding: 0.75rem;
    border: 1px solid #d1d5db;
    border-radius: 6px;
    font-size: 1rem;
    transition: border-color 0.15s ease-in-out;
  }

  input:focus {
    outline: none;
    border-color: #3b82f6;
    box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
  }

  input::placeholder {
    color: #9ca3af;
  }

  input.error {
    border-color: #ef4444;
  }

  .error-message {
    display: block;
    margin-top: 0.5rem;
    font-size: 0.875rem;
    color: #ef4444;
  }

  .submit-button {
    width: 100%;
    padding: 0.75rem 1.5rem;
    background-color: #3b82f6;
    color: white;
    border: none;
    border-radius: 6px;
    font-size: 1rem;
    font-weight: 500;
    cursor: pointer;
    transition: background-color 0.15s ease-in-out;
  }

  .submit-button:hover {
    background-color: #2563eb;
  }

  .submit-button:focus {
    outline: none;
    box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.5);
  }

  .submit-button:disabled {
    background-color: #9ca3af;
    cursor: not-allowed;
  }

  .success-message {
    margin-top: 1.5rem;
    padding: 1rem;
    background-color: #edf7ed;
    border: 1px solid #c8e6c9;
    border-radius: 6px;
    color: #1b5e20;
  }
</style>
