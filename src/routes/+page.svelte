<script lang="ts">
	import '../app.css';
	import submitBtn from '$lib/assets/images/icon-arrow.svg';

	let dayValue: number;
	let monthValue: number;
	let yearValue: number;
	let date = new Date();

	const currentYear = date.getFullYear();
	const currentMonth = date.getMonth() + 1;
	const currentDay = date.getDate();

	let yearError: string = '';
	let monthError: string = '';
	let dayError: string = '';

	function daysInMonth(year: number, month: number) {
		return new Date(year, month, 0).getDate();
	}

	function validateYear(year: number) {
		if (year > currentYear) {
			yearError = `Must be in the past.`;
			return false;
		} else if (year <= 0) {
			yearError = 'Must be valid year';
			return false;
		} else {
			yearError = '';
			return year;
		}
	}

	function validateMonth(month: number) {
		if (month > 12 || month <= 0) {
			monthError = 'Must be valid month';
		} else if (yearValue >= currentYear) {
			if (month > currentMonth) {
				monthError = `Must be in the past.`;
				return false;
			}
		} else {
			monthError = '';
			return month;
		}
	}

	function validateDay(day: number) {
		if (day < 0 || day > daysInMonth(yearValue, monthValue)) {
			dayError = `Must be valid day.`;
			return false;
		} else if (yearValue >= currentYear) {
			if (monthValue >= currentMonth) {
				if (day > currentDay) {
					dayError = `Must be in the past.`;
					return false;
				}
			}
		} else {
			dayError = '';
			return day;
		}
	}

	let ageYear: number = 0;
	let ageMonth: number = 0;
	let ageDay: number = 0;

	function handleSubmit() {
		const validatedYear = validateYear(yearValue);
		const validatedMonth = validateMonth(monthValue);
		const validatedDay = validateDay(dayValue);

		if (validatedDay && validatedMonth && validatedYear) {
			ageYear = currentYear - validatedYear;

			if (validatedMonth <= currentMonth) {
				ageMonth = currentMonth - validatedMonth;
			} else if (validatedMonth >= currentMonth) {
				ageYear--;
				ageMonth = 12 + (currentMonth - validatedMonth);
			}

			if (validatedDay <= currentDay) {
				ageDay = currentDay - validatedDay;
			} else if (validatedDay > currentDay) {
				ageMonth--;
				if (validatedMonth == currentMonth) {
					ageMonth = 12 + ageMonth;
					ageYear--;
				}
				ageDay = daysInMonth(currentYear, currentMonth) + (currentDay - validatedDay);
			}
		}
	}

	$: activateError = (trigger: string) => {
		return trigger ? 'error' : '';
	};
</script>

<section>
	<form on:submit|preventDefault={handleSubmit} novalidate>
		<div class="inputGroup">
			<label for="day">
				<span class="inputTitle {activateError(dayError)}">day</span>
				<input
					class=" {activateError(dayError)}"
					required
					bind:value={dayValue}
					type="text"
					inputmode="numeric"
					name="day"
					id="day"
					maxlength="2"
				/>
				<span class="inputError {activateError(dayError)}">{dayError}</span>
			</label>
			<label for="month">
				<span class="inputTitle {activateError(monthError)}">month</span>
				<input
					class=" {activateError(monthError)}"
					required
					bind:value={monthValue}
					type="text"
					inputmode="numeric"
					name="month"
					id="month"
					maxlength="2"
				/>
				<span class="inputError {activateError(monthError)}">{monthError}</span>
			</label>
			<label for="year">
				<span class="inputTitle {activateError(yearError)}">year</span>
				<input
					class=" {activateError(yearError)}"
					required
					bind:value={yearValue}
					type="text"
					inputmode="numeric"
					name="year"
					id="year"
					maxlength="4"
				/>
				<span class="inputError {activateError(yearError)}">{yearError}</span>
			</label>
		</div>

		<div class="submit">
			<button type="submit" name="submit" id="submit" value="Submit">
				<img src={submitBtn} alt="submit button" />
			</button>
		</div>
	</form>

	<div class="result">
		<ol>
			<li>
				<span>{ageYear === 0 ? '--' : ageYear}</span> Years
			</li>
			<li>
				<span>{ageMonth === 0 ? '--' : ageMonth}</span> Months
			</li>
			<li>
				<span>{ageDay === 0 ? '--' : ageDay}</span> Days
			</li>
		</ol>
	</div>
</section>

<style>
	:root {
		--fs-sm: 0.8rem;
		--fs-lg: 1.7rem;
		--fs-xl: clamp(3rem, 5vw + 1rem, 5rem);
	}

	section {
		width: 100%;
		max-width: 50rem;
		padding-inline: 1.2rem;
		padding-block: 3rem;
		background-color: hsl(0, 0%, 100%);
		border-radius: 2rem;
		border-bottom-right-radius: 10rem;
		box-shadow: 0px 3px 15px rgba(0, 0, 0, 0.1);
	}

	.inputGroup {
		display: flex;
		gap: 1rem;
		justify-content: center;
	}

	label {
		position: relative;
		display: flex;
		flex-direction: column;
		justify-content: center;
		gap: 0.5rem;
	}

	label input {
		font-size: var(--fs-lg);
		padding: 0px;
		padding-left: 0.8rem;
		padding-block: 0.5rem;
		width: 100%;
		max-width: 10rem;
		font-weight: 800;
		border-radius: 10px;
		border: 2px solid hsl(0, 0%, 86%);
		color: hsl(0, 0%, 8%) !important;
	}

	label .inputTitle {
		font-size: var(--fs-sm);
		color: hsl(0, 1%, 44%);
		text-transform: uppercase;
		letter-spacing: 3px;
		font-weight: 700;
	}

	label .inputError {
		height: 2rem;
		margin-bottom: 1rem;
		font-size: var(--fs-sm);
		line-height: 16px;
		font-style: italic;
	}

	label .error {
		color: hsl(0, 100%, 67%);
		border-color: hsl(0, 100%, 67%);
	}

	/* submit button */

	form .submit {
		position: relative;
		z-index: 5;
		display: flex;
		justify-content: center;
		margin-bottom: 2rem;
	}

	.submit button {
		transition: all 250ms;
		background-color: hsl(259, 100%, 65%);
		border: 0px;
		border-radius: 100%;
		width: 5rem;
		height: 5rem;
	}

	.submit img {
		width: 50%;
		height: auto;
	}

	.submit button:hover {
		background-color: hsl(259, 100%, 70%);
	}

	.submit button:active {
		background-color: hsl(259, 100%, 70%);
		scale: 0.9;
	}

	.submit::after {
		content: '';
		position: absolute;
		z-index: -1;
		background-color: hsl(0, 0%, 86%);
		inset: 0;
		margin: auto;
		height: 2px;
		width: 100%;
	}

	/* result section */

	.result {
		margin-top: 52px;
	}

	.result ol {
		padding-left: 0px;
	}
	.result li {
		list-style: none;
		font-size: var(--fs-xl);
		font-weight: 800;
		font-style: italic;
		text-transform: lowercase;
		color: hsl(0, 0%, 8%);
		white-space: nowrap;
	}

	.result span {
		color: hsl(259, 100%, 65%);
	}

	@media screen and (min-width: 660px) {
		section {
			padding-inline: 3rem;
		}

		.inputGroup {
			max-width: 80%;
			justify-content: left;
		}

		label .inputError {
			margin-bottom: 0px;
		}

		form .submit {
			justify-content: end;
			margin-bottom: 0px;
		}

		.submit button {
			width: 6rem;
			height: 6rem;
		}

		.result {
			margin-top: 0;
		}
	}
</style>
