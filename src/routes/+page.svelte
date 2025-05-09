<script lang="ts">
    let userId = 1;
    let postId = 1;
    let userData = null;
    let postTitle = '';
    let isLoading = false;
    let errorMessage = '';
  
    // Fetch user data
    async function fetchUser() {
      isLoading = true;
      errorMessage = '';
      try {
        const res = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);
        if (!res.ok) throw new Error('Failed to fetch user');
        const data = await res.json();
        userData = data;
        console.log('Fetch User:', res.status, data);
      } catch (error) {
        errorMessage = 'Failed to fetch user';
        console.error('Error:', error);
      } finally {
        isLoading = false;
      }
    }
  
    // Create a post
    async function createPost() {
      isLoading = true;
      errorMessage = '';
      try {
        const res = await fetch(`https://jsonplaceholder.typicode.com/posts`, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            title: 'New Post Title',
            body: 'This is the post content',
            userId
          })
        });
        if (!res.ok) throw new Error('Failed to create post');
        const data = await res.json();
        console.log('Create Post:', res.status, data);
      } catch (error) {
        errorMessage = 'Failed to create post';
        console.error('Error:', error);
      } finally {
        isLoading = false;
      }
    }
  
    // Update a post
    async function updatePost() {
      if (!postTitle.trim()) {
        errorMessage = 'Post title cannot be empty';
        return;
      }
      isLoading = true;
      errorMessage = '';
      try {
        const res = await fetch(`https://jsonplaceholder.typicode.com/posts/${postId}`, {
          method: 'PUT',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            id: postId,
            title: postTitle || 'Updated Title',
            body: 'Updated post body',
            userId
          })
        });
        if (!res.ok) throw new Error('Failed to update post');
        const data = await res.json();
        console.log('Update Post:', res.status, data);
      } catch (error) {
        errorMessage = 'Failed to update post';
        console.error('Error:', error);
      } finally {
        isLoading = false;
      }
    }
  
    // Delete a post
    async function deletePost() {
      isLoading = true;
      errorMessage = '';
      try {
        const res = await fetch(`https://jsonplaceholder.typicode.com/posts/${postId}`, {
          method: 'DELETE'
        });
        if (!res.ok) throw new Error('Failed to delete post');
        console.log('Delete Post:', res.status);
      } catch (error) {
        errorMessage = 'Failed to delete post';
        console.error('Error:', error);
      } finally {
        isLoading = false;
      }
    }
  </script>
  
  <div class="max-w-2xl mx-auto px-4 py-8 space-y-6">
    <h1 class="text-3xl font-bold text-gray-800">JSONPlaceholder CRUD Demo</h1>
  
    {#if errorMessage}
      <div class="bg-red-500 text-white p-3 rounded-md">
        <p>{errorMessage}</p>
      </div>
    {/if}
  
    <div class="flex flex-col sm:flex-row gap-3">
      <button on:click={fetchUser} class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-md">
        Fetch User #{userId}
      </button>
      <button on:click={createPost} class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-md">
        Create Post
      </button>
    </div>
  
    <div class="flex flex-col sm:flex-row gap-3">
      <input
        bind:value={postTitle}
        placeholder="New title for update"
        class="border px-4 py-2 rounded-md w-full sm:w-auto"
      />
      <button on:click={updatePost} class="bg-yellow-500 hover:bg-yellow-600 text-white px-4 py-2 rounded-md">
        Update Post #{postId}
      </button>
    </div>
  
    <button on:click={deletePost} class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-md">
      Delete Post #{postId}
    </button>
  
    {#if isLoading}
      <div class="mt-6 text-blue-500">
        <p>Loading...</p>
      </div>
    {/if}
  
    {#if userData}
      <div class="mt-6">
        <h2 class="text-xl font-semibold text-gray-700 mb-2">User Info</h2>
        <pre class="bg-gray-100 p-4 rounded-lg text-sm">{JSON.stringify(userData, null, 2)}</pre>
      </div>
    {/if}
  </div>
  