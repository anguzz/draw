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
	let canvasWidth = 500 // Initial canvas width
	let canvasHeight = 500 // Initial canvas height
  
	onMount(() => {
	  context = canvas.getContext('2d')
	  context.lineWidth = lineWidth
  
	  canvas.setAttribute('width', canvasWidth)
	  canvas.setAttribute('height', canvasHeight)
  
	  handleSize()
	})
  
	$: if (context) {
	  context.strokeStyle = color
	}
  
	const handleStart = ({ offsetX: x, offsetY: y }) => {
	  if (color === background) {
		context.clearRect(0, 0, canvasWidth, canvasHeight)
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
  
	function changeCanvasSize() {
	  const newWidth = parseInt(prompt("Enter a new canvas width:"));
	  const newHeight = parseInt(prompt("Enter a new canvas height:"));
  
	  if (!isNaN(newWidth) && !isNaN(newHeight)) {
		canvasWidth = newWidth;
		canvasHeight = newHeight;
  
		canvas.setAttribute('width', canvasWidth);
		canvas.setAttribute('height', canvasHeight);
  
		handleSize();
	  }
	}
  </script>
  
  <svelte:window on:resize={handleSize} />
  
  <canvas
	{canvasWidth}
	{canvasHeight}
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
  
  <button on:click={changeLineWidth}>Change Line Width</button>
  <button on:click={changeCanvasSize}>Change Canvas Size</button>
  