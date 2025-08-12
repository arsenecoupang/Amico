<script lang="ts">
	import { onMount } from 'svelte';
	import { Sparkles, Mail, Lock, User } from 'lucide-svelte';
	import { signUp, signIn } from '../utils/api';
	import AmicoLogo from './AmicoLogo.svelte';

	export let onLogin: (userData: any) => void;
	export let onNeedOnboarding: (email: string, password: string) => void;

	let loginData = {
		email: '',
		password: ''
	};

	let signupData = {
		email: '',
		password: '',
		confirmPassword: ''
	};

	let isLoading = false;
	let error = '';

	const handleLogin = async (e: Event) => {
		e.preventDefault();
		isLoading = true;
		error = '';

		try {
			const result = await signIn(loginData.email, loginData.password);
			if (result.session?.user) {
				onLogin(result.session.user);
			}
		} catch (err: any) {
			console.error('Sign in error:', err);

			let errorMessage = '로그인에 실패했습니다.';
			let shouldAutoDemo = false;

			if (err.message?.includes('Invalid login credentials')) {
				errorMessage = '이메일 또는 비밀번호가 올바르지 않습니다. 아래 데모 버튼으로 체험해보세요!';
				shouldAutoDemo = true;
			} else if (err.message?.includes('Email not confirmed')) {
				errorMessage = '이메일 인증이 완료되지 않았습니다. 데모 모드로 체험해보세요!';
				shouldAutoDemo = true;
			} else if (err.message?.includes('Failed to fetch') || err.message?.includes('network')) {
				errorMessage = '서버에 연결할 수 없습니다. 데모 모드로 체험해보세요!';
				shouldAutoDemo = true;
			}

			error = errorMessage;

			if (shouldAutoDemo) {
				setTimeout(() => {
					handleDemoLogin();
				}, 2000);
			}
		} finally {
			isLoading = false;
		}
	};

	const handleSignupStart = (e: Event) => {
		e.preventDefault();
		error = '';

		if (signupData.password !== signupData.confirmPassword) {
			error = '비밀번호가 일치하지 않습니다.';
			return;
		}

		if (signupData.password.length < 6) {
			error = '비밀번호는 최소 6자 이상이어야 합니다.';
			return;
		}

		onNeedOnboarding(signupData.email, signupData.password);
	};

	const handleDemoLogin = () => {
		const demoUser = {
			id: 'demo-user-123',
			email: 'demo@example.com',
			user_metadata: {
				nickname: '데모유저',
				personality: ['외향적', '유머러스'],
				hobbies: ['음악', '게임'],
				mbti: 'ENFP',
				group: '1학년 1반'
			}
		};
		onLogin(demoUser);
	};

	let activeTab = 'login';
</script>

<div
	class="flex min-h-screen items-center justify-center bg-gradient-to-br from-[var(--pastel-pink)] via-[var(--pastel-blue)] to-[var(--pastel-purple)] p-4"
>
	<div class="w-full max-w-md">
		<!-- Logo -->
		<div class="mb-8 text-center">
			<div
				class="mb-4 inline-flex h-16 w-16 items-center justify-center rounded-full bg-white shadow-lg"
			>
				<Sparkles class="h-8 w-8 text-[var(--soft-blue)]" />
			</div>
			<AmicoLogo size="lg" class="justify-center text-white" />
			<p class="mt-2 text-white/80">새로운 친구들과 만나보세요!</p>
		</div>

		<div class="rounded-xl bg-white p-6 shadow-xl">
			<!-- Tabs -->
			<div class="mb-6 grid grid-cols-2">
				<button
					class={`py-2 ${activeTab === 'login' ? 'border-b-2 border-blue-500 font-bold' : 'text-gray-500'}`}
					on:click={() => (activeTab = 'login')}
				>
					로그인
				</button>
				<button
					class={`py-2 ${activeTab === 'signup' ? 'border-b-2 border-blue-500 font-bold' : 'text-gray-500'}`}
					on:click={() => (activeTab = 'signup')}
				>
					회원가입
				</button>
			</div>

			{#if error}
				<div class="mb-4 rounded border border-red-200 bg-red-50 p-3 text-sm text-red-700">
					{error}
				</div>
			{/if}

			{#if activeTab === 'login'}
				<form on:submit={handleLogin} class="space-y-4">
					<div class="space-y-2">
						<label for="login-email">이메일</label>
						<div class="relative">
							<Mail class="absolute top-3 left-3 h-4 w-4 text-gray-400" />
							<input
								id="login-email"
								type="email"
								placeholder="이메일을 입력하세요"
								class="w-full rounded border px-3 py-2 pl-10"
								bind:value={loginData.email}
								required
							/>
						</div>
					</div>

					<div class="space-y-2">
						<label for="login-password">비밀번호</label>
						<div class="relative">
							<Lock class="absolute top-3 left-3 h-4 w-4 text-gray-400" />
							<input
								id="login-password"
								type="password"
								placeholder="비밀번호를 입력하세요"
								class="w-full rounded border px-3 py-2 pl-10"
								bind:value={loginData.password}
								required
							/>
						</div>
					</div>

					<button
						type="submit"
						class="w-full rounded bg-[var(--soft-blue)] py-2 text-white hover:bg-[var(--soft-blue)]/90"
						disabled={isLoading}
					>
						{isLoading ? '로그인 중...' : '로그인'}
					</button>
				</form>
			{:else}
				<form on:submit={handleSignupStart} class="space-y-4">
					<div class="space-y-2">
						<label for="signup-email">이메일</label>
						<div class="relative">
							<Mail class="absolute top-3 left-3 h-4 w-4 text-gray-400" />
							<input
								id="signup-email"
								type="email"
								placeholder="이메일을 입력하세요"
								class="w-full rounded border px-3 py-2 pl-10"
								bind:value={signupData.email}
								required
							/>
						</div>
					</div>

					<div class="space-y-2">
						<label for="signup-password">비밀번호</label>
						<div class="relative">
							<Lock class="absolute top-3 left-3 h-4 w-4 text-gray-400" />
							<input
								id="signup-password"
								type="password"
								placeholder="비밀번호를 입력하세요 (6자 이상)"
								class="w-full rounded border px-3 py-2 pl-10"
								bind:value={signupData.password}
								required
							/>
						</div>
					</div>

					<div class="space-y-2">
						<label for="signup-confirm">비밀번호 확인</label>
						<div class="relative">
							<Lock class="absolute top-3 left-3 h-4 w-4 text-gray-400" />
							<input
								id="signup-confirm"
								type="password"
								placeholder="비밀번호를 다시 입력하세요"
								class="w-full rounded border px-3 py-2 pl-10"
								bind:value={signupData.confirmPassword}
								required
							/>
						</div>
					</div>

					<button
						type="submit"
						class="w-full rounded bg-[var(--soft-green)] py-2 text-white hover:bg-[var(--soft-green)]/90"
						disabled={isLoading}
					>
						다음 단계로
					</button>
				</form>
			{/if}

			<!-- Demo Button -->
			<div class="mt-6 border-t border-gray-200 pt-6">
				<button
					class="w-full rounded bg-gradient-to-r from-[var(--soft-green)] to-[var(--soft-blue)] py-2 text-white hover:opacity-90"
					on:click={handleDemoLogin}
				>
					<User class="mr-2 inline h-4 w-4" />
					✨ 데모로 바로 시작하기 ✨
				</button>
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
				안전하고 건전한 소통을 위해<br />
				서로를 존중하며 대화해주세요 💝
			</p>
		</div>
	</div>
</div>
