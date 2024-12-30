<!-- JsonFormatter.svelte -->
<script lang="js">
  import { onMount } from 'svelte';

  let editor;
  let error = '';

  // Format and highlight JSON content
  function formatAndHighlight(content) {
    try {
      if (!content.trim()) {
        return '';
      }

      // Parse and re-stringify to validate and format
      const parsed = JSON.parse(content);
      const formatted = JSON.stringify(parsed, null, 2);
      
      // Apply syntax highlighting
      return formatted.replace(
        /("(\\u[a-zA-Z0-9]{4}|\\[^u]|[^\\"])*"(\s*:)?|\b(true|false|null)\b|-?\d+(?:\.\d*)?(?:[eE][+\-]?\d+)?)/g,
        (match) => {
          let className = 'text-blue-600'; // number
          if (/^"/.test(match)) {
            if (/:$/.test(match)) {
              className = 'text-red-600'; // key
            } else {
              className = 'text-green-600'; // string
            }
          } else if (/true|false/.test(match)) {
            className = 'text-purple-600'; // boolean
          } else if (/null/.test(match)) {
            className = 'text-gray-600'; // null
          }
          return `<span class="${className}">${match}</span>`;
        }
      );
    } catch (err) {
      if (err instanceof Error) {
        error = err.message;
      }
      return content;
    }
  }

  // Handle input changes
  function handleInput(e) {
    const content = (e.currentTarget).textContent || '';
    try {
      JSON.parse(content);
      error = '';
    } catch (err) {
      if (err instanceof Error) {
        error = err.message;
      }
    }
  }

  // Handle paste event
  function handlePaste(e) {
    e.preventDefault();
    const text = e.clipboardData?.getData('text') || '';
    
    try {
      const parsed = JSON.parse(text);
      const formatted = JSON.stringify(parsed, null, 2);
      const highlighted = formatAndHighlight(formatted);
      
      // Insert formatted content
      editor.innerHTML = highlighted;
      error = '';
      
      // Move cursor to end
      const range = document.createRange();
      const selection = window.getSelection();
      if (selection) {
        range.selectNodeContents(editor);
        range.collapse(false);
        selection.removeAllRanges();
        selection.addRange(range);
      }
    } catch (err) {
      // If invalid JSON, just insert the plain text
      editor.textContent = text;
      if (err instanceof Error) {
        error = err.message;
      }
    }
  }

  // Format on blur
  function handleBlur() {
    const content = editor.textContent || '';
    const highlighted = formatAndHighlight(content);
    editor.innerHTML = highlighted;
  }

  // Handle tab key
  function handleKeyDown(e) {
    if (e.key === 'Tab') {
      e.preventDefault();
      document.execCommand('insertText', false, '  ');
    }
  }

  onMount(() => {
    // Initial setup if needed
  });
</script>

<div class="w-full max-w-4xl p-4">
  <div class="bg-white rounded-lg shadow-sm border border-gray-200 p-4">
    <div
      bind:this={editor}
      class="min-h-[12rem] p-4 border rounded-md font-mono text-sm bg-gray-50 focus:outline-none whitespace-pre overflow-auto"
      contenteditable="true"
      on:input={handleInput}
      on:paste={handlePaste}
      on:blur={handleBlur}
      on:keydown={handleKeyDown}
      spellcheck="false"
      id="editor"
    />
    {#if error}
      <div class="text-red-500 text-sm mt-2">
        {error}
      </div>
    {/if}
  </div>
</div>

<style>
  /* Add any additional component-specific styles here */
</style>