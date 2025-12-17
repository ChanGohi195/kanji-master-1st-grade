<script lang="ts">
	import { UI } from '$lib/data/ui-text';
</script>

<svelte:head>
	<title>ダウンロード - {UI.appName}</title>
</svelte:head>

<div class="min-h-screen bg-gradient-to-b from-amber-50 to-orange-50">
	<header class="bg-white shadow-md">
		<div class="mx-auto flex max-w-lg items-center justify-between px-4 py-4">
			<a href="/" class="text-xl text-amber-500 hover:text-amber-700">← {UI.back}</a>
			<h1 class="text-2xl font-bold text-gray-800">ダウンロード</h1>
			<div class="w-16"></div>
		</div>
	</header>

	<main class="mx-auto max-w-lg px-4 py-8">
		<section class="rounded-3xl bg-white p-6 shadow-xl">
			<div class="text-center">
				<div class="mb-4 text-6xl">📦</div>
				<h2 class="mb-4 text-2xl font-bold text-gray-800">アプリをダウンロード</h2>
				<p class="mb-6 text-gray-600">
					ZIPファイルをダウンロードして、<br />ローカル環境で動かすことができます。
				</p>

				<a
					href="/kanji-master-1st-grade.zip"
					download
					class="inline-block rounded-2xl bg-gradient-to-br from-orange-500 to-amber-500 px-8 py-4 text-xl font-bold text-white shadow-lg transition-all hover:from-orange-600 hover:to-amber-600 active:scale-95"
				>
					⬇️ ZIPをダウンロード
				</a>

				<div class="mt-8 rounded-xl bg-amber-50 p-4 text-left">
					<h3 class="mb-2 font-bold text-amber-800">📖 使い方</h3>
					<ol class="list-inside list-decimal space-y-2 text-sm text-amber-700">
						<li>ZIPファイルをダウンロード</li>
						<li>解凍してフォルダを開く</li>
						<li>index.html をブラウザで開く</li>
					</ol>
					<p class="mt-3 text-xs text-amber-600">
						※ローカルサーバーで動かす場合は、解凍したフォルダで<br />
						<code class="rounded bg-amber-100 px-1">npx serve</code> を実行してください。
					</p>
				</div>
			</div>
		</section>
	</main>
</div>
