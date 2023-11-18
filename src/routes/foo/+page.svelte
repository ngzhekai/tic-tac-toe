<script lang="ts">
	// logic goes here
	import { onMount } from 'svelte';

	const winningPattern: number[][] = [
		[0, 1, 2],
		[0, 3, 6],
		[0, 4, 8],
		[1, 4, 7],
		[2, 4, 6],
		[2, 5, 8],
		[3, 4, 5],
		[6, 7, 8]
	];
	let xTurn: boolean = true;
	let count: number = 0;

	onMount(() => {
		const btnRef = document.querySelectorAll('.button-opt');
		const popupRef = document.querySelector('.popup');
		const newgameBtn = document.getElementById('new-game');
		const restartBtn = document.getElementById('restart');
		const messageRef = document.getElementById('message');

		const disableButtons = () => {
			btnRef.forEach((element) => {
				const buttonElement = element as HTMLButtonElement;
				buttonElement.disabled = true;
			});
			popupRef?.classList.remove('hide');
		};

		const enableButtons = () => {
			btnRef.forEach((element) => {
				const buttonElement = element as HTMLButtonElement;
				buttonElement.innerText = '';
				buttonElement.disabled = false;
			});
			popupRef?.classList.add('popup', 'hide');
		};

		const winFunction = (letter: string) => {
			disableButtons();

			if (letter === 'X') {
				messageRef!.innerHTML = '&#x1F389; <br> <b>X</b> Wins';
			} else {
				messageRef!.innerHTML = '&#x1F389; <br> <b>O</b> Wins';
			}
		};

		const drawFunction = () => {
			disableButtons();
			messageRef!.innerHTML = "&#x1F389; <br> It's a Draw";
		};

		newgameBtn!.addEventListener('click', () => {
			count = 0;
			enableButtons();
		});

		restartBtn!.addEventListener('click', () => {
			count = 0;
			enableButtons();
		});

		const winChecker = () => {
			for (const i of winningPattern) {
				const [index1, index2, index3] = i;
				const element1 = btnRef[index1] as HTMLButtonElement;
				const element2 = btnRef[index2] as HTMLButtonElement;
				const element3 = btnRef[index3] as HTMLButtonElement;

				const value1 = element1.innerText;
				const value2 = element2.innerText;
				const value3 = element3.innerText;

				if (value1 !== '' && value2 !== '' && value3 !== '') {
					if (value1 === value2 && value2 === value3) {
						winFunction(value1);
					}
				}
			}
		};

		const handleClick = (element: HTMLButtonElement) => {
			const buttonElement = element;
			if (xTurn) {
				buttonElement.innerText = 'X';
			} else {
				buttonElement.innerText = 'O';
			}

			buttonElement.disabled = true;
			xTurn = !xTurn;
			count = count + 1;

			if (count === 9) {
				drawFunction();
			}
			winChecker();
		};

		btnRef.forEach((element) => {
			const buttonElement = element as HTMLButtonElement;
			buttonElement.addEventListener('click', () => handleClick(buttonElement));
		});

		window.onload = () => enableButtons();
	});
</script>

<!-- markup (zero or more items) goes here -->
<div class="wrapper">
	<div class="container">
		<button class="button-opt"></button>
		<button class="button-opt"></button>
		<button class="button-opt"></button>
		<button class="button-opt"></button>
		<button class="button-opt"></button>
		<button class="button-opt"></button>
		<button class="button-opt"></button>
		<button class="button-opt"></button>
		<button class="button-opt"></button>
	</div>

	<button id="restart" class="btn">Restart</button>
</div>

<div class="popup hide">
	<p id="message">Sample Message</p>
	<button id="new-game" class="btn">New Game</button>
</div>

<style>
	/* styles go here */
	:root {
		--cell-size: 8rem;
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	:global(body) {
		margin: 0;
		padding: 0;
		height: 100vh;
		background: #dac4f7;
		font-family:
			system-ui,
			-apple-system,
			BlinkMacSystemFont,
			'Segoe UI',
			Roboto,
			Oxygen,
			Ubuntu,
			Cantarell,
			'Open Sans',
			'Helvetica Neue',
			sans-serif;
	}

	.wrapper {
		position: relative;
		width: 100%;
        height: 100%;
		margin: auto;
		display: flex;
		flex-direction: column;
		justify-content: center;
	}

	.container {
		display: grid;
		justify-content: center;
		justify-items: center;
		align-content: center;
		align-items: center;
		gap: 1rem;
		grid-template-columns: repeat(3, auto);
	}
	.button-opt {
		width: var(--cell-size);
		height: var(--cell-size);
		border: 1px solid black;
		cursor: pointer;
		font-size: 3.5rem;
		line-height: 1;
		box-shadow: 10px 10px 10px #c4a7eb;
		border-radius: 8px;
	}

	.btn {
		display: block;
		background-color: #f7b5b8;
		font-size: 1.3rem;
		border-radius: 8px;
		padding: 0.5rem 1rem;
		width: fit-content;
		border: none;
		margin: 3rem auto 0 auto;
		box-shadow: 2px 2px 8px #c4a7eb;
	}

	.btn:hover {
		background-color: #f4989c;
	}

	button:hover {
		transition-property: all;
		transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
		transition-duration: 170ms;
		transition-delay: 35ms;
		transform: scale(1.1);
	}

	.popup {
		background: #dac4f7;
		opacity: 95%;
		top: 0;
		left: 0;
		height: 100%;
		width: 100%;
		display: flex;
		flex-direction: column;
		z-index: 2;
		align-items: center;
		position: absolute;
		justify-content: center;
		font-size: 2rem;
		gap: 1rem;
	}
	#message {
		text-align: center;
		font-size: 2rem;
	}

	.hide {
		display: none;
	}
</style>
