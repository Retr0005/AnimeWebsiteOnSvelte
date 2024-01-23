<script>
	import '../app.css';
	import { AppShell, AppBar } from '@skeletonlabs/skeleton';
</script>

<!-- App Shell -->
<AppShell>
	<svelte:fragment slot="header">
		<!-- App Bar -->
		<AppBar gridColumns="grid-cols-1" slotDefault="place-self-center" slotTrail="place-content-end">
			<strong class="text-xl uppercase">Fapello</strong>
		</AppBar>
	</svelte:fragment>
	<!-- Page Route Content -->
	<slot />
	<svelte:fragment slot="pageFooter">
		<AppBar gridColumns="grid-cols-3" slotDefault="place-self-center" slotTrail="place-content-end">
			
		<svelte:fragment slot="lead">
				<div class="mt-4">
				All Rights Reserved, all files on this site are the <br>
				 property of their respective and rightful owners. 
				</div>
		</svelte:fragment>
			<strong class="text-xl uppercase">Fapello</strong>
			<svelte:fragment slot="trail">
				<div class="mt-4">
                    Disclaimer: This site does not store any files on its server.<br>
					 All contents are provided by non-affiliated third parties.
                </div>
			</svelte:fragment>

			
		</AppBar>
	</svelte:fragment>
</AppShell>
