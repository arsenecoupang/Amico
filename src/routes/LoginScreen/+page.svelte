<script>
	import AmicoLogo from '../../AmicoLogo.svelte';
	import { Sparkles } from 'lucide-svelte';

	// 현재 선택된 탭 ("login" 또는 "signup")
	let selectedTab = 'login';

	// 로그인 입력값
	let email = '';
	let password = '';

	// 회원가입 입력값
	let signupEmail = '';
	let signupPassword = '';
	let signupConfirmPassword = '';

	// 로그인 처리
	function handleLogin(e) {
		e.preventDefault();
		console.log('로그인 시도:', email, password);
		// 여기에 실제 로그인 로직 추가
	}

	// 회원가입 처리
	function handleSignup(e) {
		e.preventDefault();
		if (signupPassword !== signupConfirmPassword) {
			alert('비밀번호가 일치하지 않습니다!');
			return;
		}
		console.log('회원가입 시도:', signupEmail, signupPassword);
		// 여기에 실제 회원가입 로직 추가
	}

	// 데모 시작
	function startDemo() {
		console.log('데모 시작');
		// 여기에 데모 모드 로직 추가
	}

	//style
	let loginInputStyle =
		'border-input bg-muted flex h-9 w-full rounded-md border px-3 py-1 pl-10 text-base md:text-sm';
</script>

<!--전체 스크린-->
<div
	class="flex min-h-screen items-center justify-center bg-gradient-to-br from-[var(--pastel-pink)] via-[var(--pastel-blue)] to-[var(--pastel-purple)] p-4"
>
	<div>
		<!-- 로고 소개 -->
		<div class="mb-8 w-full max-w-md text-center">
			<div
				class="mb-4 inline-flex h-16 w-16 items-center justify-center rounded-full bg-white shadow-lg"
			>
				<Sparkles class="h-8 w-8 text-[var(--soft-blue)]" />
			</div>
			<AmicoLogo size="lg" className="text-white justify-center" showIcon={true} />
			<p class="mt-2 text-white/80">새로운 친구들과 만나보세요!</p>
		</div>
		<!--		로그인 및 회원가입 -->
		<div
			class="mx-auto w-full max-w-3xl min-w-[320px] overflow-hidden rounded-2xl bg-white p-6 shadow-2xl"
		>
			<!-- 탭 버튼 -->
			<div
				id="LoginSelect"
				class="mb-6 grid h-9 w-full grid-cols-2 items-center justify-center rounded-xl bg-muted p-[3px] text-muted-foreground"
			>
				<button
					class={`inline-flex items-center justify-center gap-1.5 rounded-xl border border-transparent px-2 py-1 text-sm font-medium transition-colors
        ${selectedTab === 'login' ? 'bg-card text-foreground' : 'text-muted-foreground'}
      `}
					on:click={() => (selectedTab = 'login')}
				>
					로그인
				</button>
				<button
					class={`inline-flex items-center justify-center gap-1.5 rounded-xl border border-transparent px-2 py-1 text-sm font-medium transition-colors
        ${selectedTab === 'signup' ? 'bg-card text-foreground' : 'text-muted-foreground'}
      `}
					on:click={() => (selectedTab = 'signup')}
				>
					회원가입
				</button>
			</div>

			<!-- 로그인 탭 -->
			{#if selectedTab === 'login'}
				<form class="space-y-4" on:submit={handleLogin}>
					<div class="space-y-2">
						<label class="flex items-center gap-2 text-sm font-medium" for="login-email"
							>이메일</label
						>
						<div class="relative">
							<svg
								xmlns="http://www.w3.org/2000/svg"
								width="24"
								height="24"
								viewBox="0 0 24 24"
								fill="none"
								stroke="currentColor"
								stroke-width="2"
								stroke-linecap="round"
								stroke-linejoin="round"
								class="lucide lucide-mail absolute top-3 left-3 h-4 w-4 text-gray-400"
								aria-hidden="true"
								><path d="m22 7-8.991 5.727a2 2 0 0 1-2.009 0L2 7"></path><rect
									x="2"
									y="4"
									width="20"
									height="16"
									rx="2"
								></rect></svg
							>
							<input
								type="email"
								id="login-email"
								placeholder="이메일을 입력하세요"
								class={loginInputStyle}
								bind:value={email}
								required
							/>
						</div>
					</div>

					<div class="space-y-2">
						<label class="flex items-center gap-2 text-sm font-medium" for="login-password"
							>비밀번호</label
						>
						<div class="relative">
							<svg
								xmlns="http://www.w3.org/2000/svg"
								width="24"
								height="24"
								viewBox="0 0 24 24"
								fill="none"
								stroke="currentColor"
								stroke-width="2"
								stroke-linecap="round"
								stroke-linejoin="round"
								class="lucide lucide-lock absolute top-3 left-3 h-4 w-4 text-gray-400"
								aria-hidden="true"
								><rect width="18" height="11" x="3" y="11" rx="2" ry="2"></rect><path
									d="M7 11V7a5 5 0 0 1 10 0v4"
								></path></svg
							>
							<input
								type="password"
								id="login-password"
								placeholder="비밀번호를 입력하세요"
								class={loginInputStyle}
								bind:value={password}
								required
							/>
						</div>
					</div>

					<button
						type="submit"
						class="h-9 w-full rounded-md bg-[var(--soft-blue)] px-4 py-2 text-sm font-medium text-white hover:bg-[var(--soft-blue)]/90"
					>
						로그인
					</button>
				</form>
			{/if}

			<!-- 회원가입 탭 -->
			{#if selectedTab === 'signup'}
				<form class="space-y-4" on:submit={handleLogin}>
					<div class="space-y-2">
						<label class="flex items-center gap-2 text-sm font-medium" for="login-email"
							>이메일</label
						>
						<div class="relative">
							<svg
								xmlns="http://www.w3.org/2000/svg"
								width="24"
								height="24"
								viewBox="0 0 24 24"
								fill="none"
								stroke="currentColor"
								stroke-width="2"
								stroke-linecap="round"
								stroke-linejoin="round"
								class="lucide lucide-mail absolute top-3 left-3 h-4 w-4 text-gray-400"
								aria-hidden="true"
								><path d="m22 7-8.991 5.727a2 2 0 0 1-2.009 0L2 7"></path><rect
									x="2"
									y="4"
									width="20"
									height="16"
									rx="2"
								></rect></svg
							>
							<input
								type="email"
								id="login-email"
								placeholder="이메일을 입력하세요"
								class={loginInputStyle}
								bind:value={signupEmail}
								required
							/>
						</div>
					</div>

					<div class="space-y-2">
						<label class="flex items-center gap-2 text-sm font-medium" for="login-password"
							>비밀번호</label
						>
						<div class="relative">
							<svg
								xmlns="http://www.w3.org/2000/svg"
								width="24"
								height="24"
								viewBox="0 0 24 24"
								fill="none"
								stroke="currentColor"
								stroke-width="2"
								stroke-linecap="round"
								stroke-linejoin="round"
								class="lucide lucide-lock absolute top-3 left-3 h-4 w-4 text-gray-400"
								aria-hidden="true"
								><rect width="18" height="11" x="3" y="11" rx="2" ry="2"></rect><path
									d="M7 11V7a5 5 0 0 1 10 0v4"
								></path></svg
							>
							<input
								type="password"
								id="login-password"
								placeholder="비밀번호를 입력하세요"
								class={loginInputStyle}
								bind:value={signupPassword}
								required
							/>
						</div>
					</div>
					<div class="space-y-2">
						<label class="flex items-center gap-2 text-sm font-medium" for="login-password"
							>비밀번호 확인</label
						>
						<div class="relative">
							<svg
								xmlns="http://www.w3.org/2000/svg"
								width="24"
								height="24"
								viewBox="0 0 24 24"
								fill="none"
								stroke="currentColor"
								stroke-width="2"
								stroke-linecap="round"
								stroke-linejoin="round"
								class="lucide lucide-lock absolute top-3 left-3 h-4 w-4 text-gray-400"
								aria-hidden="true"
								><rect width="18" height="11" x="3" y="11" rx="2" ry="2"></rect><path
									d="M7 11V7a5 5 0 0 1 10 0v4"
								></path></svg
							>
							<input
								type="password"
								id="login-password"
								placeholder="비밀번호를 다시 입력하세요"
								class={loginInputStyle}
								bind:value={signupConfirmPassword}
								required
							/>
						</div>
					</div>

					<button
						data-slot="button"
						class="[&amp;_svg]:pointer-events-none [&amp;_svg:not([class*='size-'])]:size-4 [&amp;_svg]:shrink-0 has-[&gt;svg]:px-3 inline-flex h-9 w-full shrink-0 items-center justify-center gap-2 rounded-md bg-[var(--soft-green)] px-4 py-2 text-sm font-medium whitespace-nowrap text-primary-foreground transition-all outline-none hover:bg-[var(--soft-green)]/90 focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50 disabled:pointer-events-none disabled:opacity-50 aria-invalid:border-destructive aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40"
						type="submit">다음 단계로</button
					>
				</form>
			{/if}

			<!-- 데모 시작 -->
			<div class="mt-6 border-t border-gray-200 pt-6">
				<button
					data-slot="button"
					class="[&amp;_svg]:pointer-events-none [&amp;_svg:not([class*='size-'])]:size-4 [&amp;_svg]:shrink-0 has-[&gt;svg]:px-3 inline-flex h-9 w-full shrink-0 items-center justify-center gap-2 rounded-md border-0 bg-gradient-to-r from-[var(--soft-green)] to-[var(--soft-blue)] px-4 py-2 text-sm font-medium whitespace-nowrap text-white transition-all outline-none hover:bg-accent hover:text-accent-foreground hover:opacity-90 focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50 disabled:pointer-events-none disabled:opacity-50 aria-invalid:border-destructive aria-invalid:ring-destructive/20 dark:border-input dark:bg-input/30 dark:hover:bg-input/50 dark:aria-invalid:ring-destructive/40"
					><svg
						xmlns="http://www.w3.org/2000/svg"
						width="24"
						height="24"
						viewBox="0 0 24 24"
						fill="none"
						stroke="currentColor"
						stroke-width="2"
						stroke-linecap="round"
						stroke-linejoin="round"
						class="lucide lucide-user mr-2 h-4 w-4"
						aria-hidden="true"
						><path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2"></path><circle cx="12" cy="7" r="4"
						></circle></svg
					>✨ 데모로 바로 시작하기 ✨</button
				>
				<p class="mt-2 text-center text-xs text-gray-500">회원가입 없이 모든 기능을 체험해보세요</p>
				<div
					class="mt-4 rounded-lg border border-blue-200 bg-gradient-to-r from-blue-50 to-green-50 p-3"
				>
					<p class="mb-1 text-xs font-medium text-blue-700">🚀 즉시 체험 가능!</p>
					<p class="text-xs text-blue-600">
						로그인 문제가 있다면 위의 "데모로 바로 시작하기" 버튼으로<br />
						Amico의 모든 기능을 즉시 체험할 수 있습니다.
					</p>
				</div>
			</div>
		</div>
		<div class="mt-6 text-center">
			<p class="text-sm text-gray-600">
				안전하고 건전한 소통을 위해<br />서로를 존중하며 대화해주세요 💝
			</p>
		</div>
	</div>
</div>
