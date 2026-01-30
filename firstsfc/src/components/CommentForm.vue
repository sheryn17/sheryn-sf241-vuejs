<template>
  <div class="comment-container">
    <h2>Leave a Comment</h2>
    <form @submit.prevent="submitComment">
      <div class="form-group">
        <label for="name">Name:</label>
        <input 
          type="text" 
          id="name" 
          v-model.trim="name" 
          required 
          class="form-control"
          placeholder="Your name"
        >
      </div>
      
      <div class="form-group">
        <label for="comment">Comment:</label>
        <textarea 
          id="comment" 
          v-model.trim="comment" 
          required 
          class="form-control"
          placeholder="Write something..."
        ></textarea>
      </div>

      <button 
        type="submit" 
        class="btn btn-primary" 
        :disabled="isSubmitting"
      >
        {{ isSubmitting ? 'Submitting...' : 'Submit' }}
      </button>

      <div class="status-message">
        <p v-if="submissionStatus" :class="{ 'error': isError }">
          {{ submissionStatus }}
        </p>
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { supabase } from '../lib/supabaseClient';

const name = ref('');
const comment = ref('');
const submissionStatus = ref(null);
const isSubmitting = ref(false);
const isError = ref(false);

const tableName = 'comments';

async function submitComment() {
  if (isSubmitting.value) return;

  isSubmitting.value = true;
  isError.value = false;
  submissionStatus.value = "Sending your comment...";

  try {
    const { error } = await supabase
      .from(tableName)
      .insert([{ name: name.value, comment: comment.value }]);

    if (error) throw error;

    submissionStatus.value = "Comment submitted successfully!";
    name.value = ''; 
    comment.value = '';
    
    // Clear the success message after 3 seconds
    setTimeout(() => { submissionStatus.value = null; }, 3000);

  } catch (err) {
    console.error("Submission error:", err);
    isError.value = true;
    submissionStatus.value = "Error: Could not save comment.";
  } finally {
    isSubmitting.value = false;
  }
}
</script>

<style scoped>
.comment-container {
  max-width: 500px;
  margin: 0 auto;
}

.form-group {
  margin-bottom: 1rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: bold;
}

.form-control {
  width: 100%;
  padding: 0.6rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-family: inherit;
}

.btn {
  padding: 0.6rem 1.2rem;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: opacity 0.2s;
}

.btn:disabled {
  background-color: #6c757d;
  cursor: not-allowed;
}

.status-message {
  height: 20px; /* Reserves space so text appearing doesn't move the form */
  margin-top: 10px;
  font-size: 0.9rem;
}

.error {
  color: #dc3545;
}
</style>
