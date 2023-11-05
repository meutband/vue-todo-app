<script>

export default{}
</script>

<template>
	<ul class="nav">
		<li class="nav-item">
			<router-link class="nav-link active" to="/">Home</router-link>
		</li>
		<li class="nav-item">
			<router-link class="nav-link active" to="/about">About</router-link>
		</li>
	</ul>
	<router-view />
</template>

<style>
</style>
