<script>
	import { onMount } from "svelte";

	import axios from "axios";
	let form;
	let alertbar;
	let alertbarError;
	let ehrIdInput;
	let compoInput;

	let writtenCompoUid = "";

	const axiosAPI = axios.create({
		baseURL: "http://localhost:8080/ehrbase/rest/ecis/v1",
	});

	onMount(async () => {
		ehrIdInput.value = "92fa4396-057d-4d1f-b480-9a83efd0239e";

		form.cdr = axiosAPI;
		form.template = "AddictionAlcoholTemplate";

		form.addEventListener("submit", (event) => {
			// reset bars, if set beforehand
			alertbar.removeAttribute("open");
			alertbarError.removeAttribute("open");

			form.ehr = ehrIdInput.value;

			form.post(event.detail)
				.then((r) => {
					//console.log(JSON.stringify(r));
					writtenCompoUid = r.data.compositionUid;
					alertbar.setAttribute("open", "");
				})
				.catch((error) => {
					console.error("Error:", error);
					alertbarError.setAttribute("open", "");
				});
		});

		/* form.addEventListener("load", (event) => {
			var uuid = compoInput;
			const composition = form.get(uuid);
			console.log(composition);
		}); */

		async function retrieve() {
			var uuid = compoInput;
			const composition = await form.get(uuid);
			console.log(composition);
			console.log("yeah");
		}
	});

	async function retrieve() {
		var uuid = compoInput;
		//const composition = await form.get(uuid);
		//console.log(composition);
		console.log("yeah");
	}
</script>

<main>
	<h1>Form</h1>

	<p>This is an openEHR form app built with Medblocks UI.</p>

	<p>Set EHR ID:</p>
	<sl-input bind:this={ehrIdInput} placeholder="EHR ID" clearable />

	<div>
		<h2>Template: Addiction Alcohol</h2>

		<sl-tab-group>
			<sl-tab slot="nav" panel="write">Write</sl-tab>
			<sl-tab slot="nav" panel="read">Read</sl-tab>

			<sl-tab-panel name="write">
				<mb-form bind:this={form}>
					<mb-context path="result_report/category" />
					<!-- <mb-input path="result_report/context/report_id" label="Report ID" />
				<mb-input path="result_report/context/status" label="Status" /> -->
					<mb-context path="result_report/context/start_time" />
					<mb-context path="result_report/context/setting" />
					<mb-input
						path="result_report/alcohol_use:0/any_event/consumption_details:0/form"
						label="Form"
					/>
					<!-- <mb-quantity
					path="result_report/alcohol_use:0/any_event/consumption_details:0/amount/standard_drinks"
					label="Standard Drinks"
				>
					<mb-unit unit="/h" label="/h" />
					<mb-unit unit="/d" label="/d" />
					<mb-unit unit="/wk" label="/wk" />
					<mb-unit unit="/mo" label="/mo" />
				</mb-quantity> -->
					<!-- <mb-quantity
					path="result_report/alcohol_use:0/any_event/consumption_details:0/amount/grams_consumed"
					label="Grams Consumed"
				>
					<mb-unit unit="gm/d" label="gm/d" />
					<mb-unit unit="gm/wk" label="gm/wk" />
				</mb-quantity> -->
					<!-- <mb-input
					path="result_report/alcohol_use:0/any_event/consumption_details:0/triggers"
					label="Triggers"
				/> -->
					<!-- <mb-input
					path="result_report/alcohol_use:0/any_event/consumption_details:0/context"
					label="Context"
				/> -->
					<!-- <mb-input
					path="result_report/alcohol_use:0/any_event/behavioural_issues"
					label="Behavioural Issues"
				/> -->
					<!-- <mb-input
					path="result_report/alcohol_use:0/any_event/comment"
					label="Comment"
				/> -->
					<mb-context
						path="result_report/alcohol_use:0/any_event/time"
					/>
					<mb-input
						path="result_report/alcohol_use:0/specified_day:0/consumption_details:0/form"
						label="Form"
					/>
					<mb-quantity
						path="result_report/alcohol_use:0/specified_day:0/consumption_details:0/amount/standard_drinks"
						label="Standard Drinks"
						default="/wk"
					>
						<mb-unit unit="/h" label="/h" />
						<mb-unit unit="/d" label="/d" />
						<mb-unit unit="/wk" label="/wk" />
						<mb-unit unit="/mo" label="/mo" />
					</mb-quantity>
					<!-- <mb-quantity
					path="result_report/alcohol_use:0/specified_day:0/consumption_details:0/amount/grams_consumed"
					label="Grams Consumed"
				>
					<mb-unit unit="gm/d" label="gm/d" />
					<mb-unit unit="gm/wk" label="gm/wk" />
				</mb-quantity> -->
					<!-- <mb-input
					path="result_report/alcohol_use:0/specified_day:0/consumption_details:0/triggers"
					label="Triggers"
				/> -->
					<!-- <mb-input
					path="result_report/alcohol_use:0/specified_day:0/consumption_details:0/context"
					label="Context"
				/> -->
					<mb-input
						path="result_report/alcohol_use:0/specified_day:0/behavioural_issues"
						label="Behavioural Issues"
					/>
					<!-- <mb-input
					path="result_report/alcohol_use:0/specified_day:0/comment"
					label="Comment"
				/> -->
					<mb-context
						path="result_report/alcohol_use:0/specified_day:0/time"
					/>

					<mb-quantity
						path="result_report/alcohol_use:0/standard_drink_definition"
						label="Standard Drink Definition"
						default="gm"
					>
						<mb-unit unit="gm" label="gm" />
					</mb-quantity>
					<mb-quantity
						path="result_report/alcohol_use:0/average_use_frequency"
						label="Average Use Frequency"
						default="/wk"
					>
						<mb-unit unit="/d" label="/d" />
						<mb-unit unit="/wk" label="/wk" />
						<mb-unit unit="/mo" label="/mo" />
					</mb-quantity>
					<mb-context path="result_report/alcohol_use:0/subject" />
					<mb-context path="result_report/alcohol_use:0/language" />
					<mb-context path="result_report/alcohol_use:0/encoding" />
					<mb-context path="result_report/composer" />
					<mb-context path="result_report/language" />
					<mb-context path="result_report/territory" />

					<mb-submit>Submit</mb-submit>
				</mb-form>
			</sl-tab-panel>
			<sl-tab-panel name="read">
				<p>Set Composition ID:</p>
				<sl-input
					bind:this={compoInput}
					placeholder="Composition ID"
					clearable
				/>
				<sl-button on:click={retrieve()}>Retrieve</sl-button>
			</sl-tab-panel>
			<sl-alert bind:this={alertbar} type="success">
				<sl-icon slot="icon" name="check2-circle" />
				<strong>Your changes have been saved</strong><br />
				Composition UID: {writtenCompoUid}
			</sl-alert>
			<sl-alert bind:this={alertbarError} type="danger">
				<sl-icon slot="icon" name="exclamation-octagon" />
				<strong>Error!</strong><br />
				There was an error!
			</sl-alert>
		</sl-tab-group>
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
