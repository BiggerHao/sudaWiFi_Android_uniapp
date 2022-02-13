<script>
	export default {
		onLaunch: function() {
			console.log('App Launch');
			let value = uni.getStorageSync('accInfo');
			if (value) {
				uni.navigateTo({
					url: 'login'
				});
			};
		}
	}
</script>

<style>
</style>
