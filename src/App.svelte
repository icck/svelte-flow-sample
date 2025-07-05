<script lang="ts">
  import { SvelteFlow, Controls, Background, MiniMap, type Node, type Edge } from '@xyflow/svelte';
  import '@xyflow/svelte/dist/style.css';

  let nodes = $state.raw<Node[]>([
    {
      id: '1',
      type: 'input',
      data: { label: 'Input Node' },
      position: { x: 250, y: 5 }
    },
    { id: '2', data: { label: 'Default Node' }, position: { x: 100, y: 100 } },
    { id: '3', data: { label: 'Output Node' }, position: { x: 400, y: 100 } },
    { id: '4', data: { label: 'Another Node' }, position: { x: 400, y: 200 } }
  ]);

  let edges = $state.raw<Edge[]>([
    { id: 'e1-2', source: '1', target: '2', label: 'this is an edge label' },
    { id: 'e1-3', source: '1', target: '3' },
    { id: 'e3-4', source: '3', target: '4', animated: true, label: 'animated edge' }
  ]);
</script>

<div style="height:100vh">
  <SvelteFlow {nodes} {edges} fitView>
    <Controls />
    <Background />
    <MiniMap />
  </SvelteFlow>
</div>
