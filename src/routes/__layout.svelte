<script>
	import '../app.css';
</script>

<main>
	<slot />
</main>

<footer>
	<p>
		We thank our sponsors: the Climate and Clean Air Coalition and NASA.
		Visualization assistance from Cypress River Advisors is also gratefully
		acknowledged. Development of this web tool benefited enormously from the
		work of volunteer Duke University undergraduates
		(Krista Stark, Jared Junkin, Maxwell Silverstein, Rithik Castelino,
		and Alex Glick) as well as graduate students working towards their degrees
		(Karl Seltzer, Muye Ru, Gray Li, and Longyi Yang). Suggestions and comments
		appreciated (send to drew.shindell@duke.edu).
	</p>
</footer>

<style>
	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		width: 100%;
		max-width: 1024px;
		margin: 0 auto;
		box-sizing: border-box;
	}

	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 40px;
	}

	footer a {
		font-weight: bold;
	}

	@media (min-width: 480px) {
		footer {
			padding: 40px 0;
		}
	}
</style>
