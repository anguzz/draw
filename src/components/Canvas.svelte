<script>
	import { onMount } from 'svelte'
  
	export let width = 500
	export let height = 500
	export let color = '#333'
	export let background = '#fff'
  
	let canvas
	let context
	let isDrawing
	let start
  
	let t, l
	let lineWidth = 5 // Initial line width
  
	onMount(() => {
	  context = canvas.getContext('2d')
	  context.lineWidth = lineWidth
  
	  handleSize()
	})
  
	$: if (context) {
	  context.strokeStyle = color
	}
  
	const handleStart = ({ offsetX: x, offsetY: y }) => {
	  if (color === background) {
		context.clearRect(0, 0, width, height)
	  } else {
		isDrawing = true
		start = { x, y }
	  }
	}
  
	const handleEnd = () => {
	  isDrawing = false
	}
  
	const handleMove = ({ offsetX: x1, offsetY: y1 }) => {
	  if (!isDrawing) return
  
	  const { x, y } = start
	  context.beginPath()
	  context.moveTo(x, y)
	  context.lineTo(x1, y1)
	  context.closePath()
	  context.stroke()
  
	  start = { x: x1, y: y1 }
	}
  
	const handleSize = () => {
	  const { top, left } = canvas.getBoundingClientRect()
	  t = top
	  l = left
	}
  
	function changeLineWidth() {
	  const newLineWidth = parseInt(prompt("Enter a new line width:"));
  
	  if (!isNaN(newLineWidth)) {
		lineWidth = newLineWidth;
		context.lineWidth = lineWidth;
	  }
	}
  </script>
  
  <svelte:window on:resize={handleSize} />
  

  
  <button on:click={changeLineWidth}>Change brush width</button>
<canvas
				{width}
				{height}
				style:background
				bind:this={canvas} 
				on:mousedown={handleStart}	
				on:touchstart={e => {
					const { clientX, clientY } = e.touches[0]
					handleStart({
						offsetX: clientX - l,
						offsetY: clientY - t
					})
				}}	
				on:mouseup={handleEnd}				
				on:touchend={handleEnd}				
				on:mouseleave={handleEnd}
				on:mousemove={handleMove}
				on:touchmove={e => {
					const { clientX, clientY } = e.touches[0]
					handleMove({
						offsetX: clientX - l,
						offsetY: clientY - t
					})
				}}
				/>