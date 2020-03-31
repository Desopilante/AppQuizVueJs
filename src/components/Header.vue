<template>
	<div>
		<b-nav>
			<b-nav-item disabled>Question</b-nav-item>
			<b-nav-item disabled>{{numCorrect}}/{{numTotal}}</b-nav-item>
		</b-nav>
	</div>
</template>

<script>
	export default {
		props: {
			numCorrect: Number,
			numTotal: Number
		}
	}
</script>