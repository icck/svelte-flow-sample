<script lang="ts">
  import { SvelteFlow, Controls, Background, type Node, type Edge } from '@xyflow/svelte';
  import '@xyflow/svelte/dist/style.css';

  // Initial nodes for the flow
  let nodes: Node[] = $state([
    {
      id: '1',
      type: 'input',
      data: { label: 'Start' },
      position: { x: 250, y: 5 }
    },
    { id: '2', data: { label: 'Step 1' }, position: { x: 250, y: 100 } },
    { id: '3', data: { label: 'Step 2' }, position: { x: 100, y: 200 } },
    { id: '4', data: { label: 'Step 3' }, position: { x: 400, y: 200 } },
    {
      id: '5',
      type: 'output',
      data: { label: 'End' },
      position: { x: 250, y: 300 }
    }
  ]);

  // Initial edges connecting the nodes
  const edges: Edge[] = [
    { id: 'e1-2', source: '1', target: '2', animated: true },
    { id: 'e2-3', source: '2', target: '3', animated: true },
    { id: 'e2-4', source: '2', target: '4', animated: true },
    { id: 'e3-5', source: '3', target: '5', animated: true },
    { id: 'e4-5', source: '4', target: '5', animated: true }
  ];

  let isRunning = $state(false);

  // Helper function to only reset styles without affecting the run state
  function _resetNodeStyles() {
    nodes = nodes.map((n) => {
      // eslint-disable-next-line @typescript-eslint/no-unused-vars
      const { style, ...rest } = n;
      return rest;
    });
  }

  // Function to sleep for a given time
  const sleep = (ms: number) => new Promise((res) => setTimeout(res, ms));

  // Function to run the visual process
  async function runProcess() {
    if (isRunning) return;
    isRunning = true;

    _resetNodeStyles();
    await sleep(500);

    const processOrder = ['1', '2', '3', '4', '5'];

    for (const nodeId of processOrder) {
      if (!isRunning) {
        break; // Exit if reset was clicked
      }

      // Highlight the current node by creating a new array
      nodes = nodes.map((n) => {
        if (n.id === nodeId) {
          return { ...n, style: 'background: #ff4757; color: white; border: 1px solid #ff4757;' };
        }
        return n;
      });

      // Wait for a second
      await sleep(1000);
    }

    // Only do the final reset if the process was not interrupted
    if (isRunning) {
      await sleep(2000);
      _resetNodeStyles();
    }
    
    isRunning = false;
  }

  // Function to reset node styles and stop the process
  function resetStyles() {
    isRunning = false; // This will stop the loop in runProcess
    _resetNodeStyles();
  }
</script>

<main>
  <div style="height: 500px; border: 1px solid #eee;">
    <SvelteFlow bind:nodes {edges}>
      <Controls />
      <Background />
    </SvelteFlow>
  </div>
  <div class="controls">
    <button onclick={runProcess} disabled={isRunning}>
      {#if isRunning}Running...{:else}Start Process{/if}
    </button>
    <button onclick={resetStyles}>Reset</button>
  </div>
</main>

<style>
  main {
    font-family: sans-serif;
  }

  .controls {
    margin-top: 10px;
    display: flex;
    gap: 10px;
  }

  button {
    padding: 8px 16px;
    border: none;
    background-color: #007bff;
    color: white;
    cursor: pointer;
    border-radius: 4px;
  }

  button:hover {
    background-color: #0056b3;
  }
</style>
