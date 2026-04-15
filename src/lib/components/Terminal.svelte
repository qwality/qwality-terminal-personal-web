<script>
	import { tick } from 'svelte';

	const COMMANDS = {
		help: () => [
			'Available commands:',
			'  help        - show this message',
			'  about       - about qwality',
			'  skills      - technical skills',
			'  projects    - list of projects',
			'  contact     - contact info',
			'  clear       - clear terminal',
		],
		about: () => [
			'> qwality',
			'  Developer. Builder. Sovereign.',
			'',
			'  I build things on the decentralized web.',
			'  Web3 | SvelteKit | Python | IPFS',
		],
		skills: () => [
			'> skills',
			'  Frontend  : SvelteKit, Tailwind',
			'  Web3      : Ethereum, IPFS, ENS, SIWE',
			'  Backend   : FastAPI, Python',
			'  Sec       : Post-quantum cryptography',
		],
		projects: () => [
			'> projects',
			'  [1] qwality-terminal-personal-web  - this site',
			'  more coming soon...',
		],
		contact: () => [
			'> contact',
			'  ETH: connect your wallet',
			'  ENS: qwality.eth (coming soon)',
		],
		clear: () => null,
	};

	let history = $state([
		{ type: 'output', lines: ['Welcome to qwality.sh v0.1.0', 'Type "help" to get started.', ''] }
	]);
	let input = $state('');
	let inputEl;

	async function handleSubmit() {
		const cmd = input.trim().toLowerCase();
		if (!cmd) return;

		history.push({ type: 'input', text: cmd });

		if (cmd === 'clear') {
			history = [];
		} else {
			const result = COMMANDS[cmd] ? COMMANDS[cmd]() : [`command not found: ${cmd}`, 'Type "help" for available commands.'];
			history.push({ type: 'output', lines: [...result, ''] });
		}

		input = '';
		await tick();
		inputEl?.scrollIntoView({ behavior: 'smooth' });
	}

	function handleKeydown(e) {
		if (e.key === 'Enter') handleSubmit();
	}
</script>

<div
	class="min-h-screen bg-black text-green-400 font-mono p-4 md:p-8 cursor-text"
	role="button"
	tabindex="-1"
	onclick={() => inputEl?.focus()}
	onkeydown={null}
>
	<div class="max-w-3xl mx-auto">
		<!-- History -->
		{#each history as entry}
			{#if entry.type === 'input'}
				<div class="flex gap-2 my-1">
					<span class="text-cyan-400 select-none">qwality@web:~$</span>
					<span>{entry.text}</span>
				</div>
			{:else}
				<div class="my-1 pl-4 text-green-300">
					{#each entry.lines as line}
						<div class="leading-relaxed">{line || '\u00A0'}</div>
					{/each}
				</div>
			{/if}
		{/each}

		<!-- Input line -->
		<div class="flex gap-2 mt-1">
			<span class="text-cyan-400 select-none">qwality@web:~$</span>
			<input
				bind:this={inputEl}
				bind:value={input}
				onkeydown={handleKeydown}
				type="text"
				class="bg-transparent outline-none flex-1 text-green-400 caret-green-400"
				autocomplete="off"
				autocorrect="off"
				autocapitalize="off"
				spellcheck="false"
				autofocus
			/>
		</div>
	</div>
</div>
