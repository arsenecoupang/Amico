<script>
	import AmicoLogo from '../../AmicoLogo.svelte';
	import {
		Music,
		BookOpen,
		Gamepad2,
		Heart,
		Camera,
		PenTool,
		Search,
		Plus,
		ChevronRight
	} from 'lucide-svelte';

	// 단계 관리
	let step = 1;
	let isLoading = false;
	let error = '';
	let searchTerm = '';
	let selectedCategory = '학년/반';
	let showCustomInput = false;
	let customGroup = '';

	let data = {
		nickname: '',
		personality: [],
		hobbies: [],
		mbti: '',
		group: ''
	};

	const personalityOptions = [
		{
			id: 'outgoing',
			label: '외향적',
			icon: '😊',
			color: 'bg-[var(--pastel-pink)]'
		},
		{
			id: 'quiet',
			label: '조용한',
			icon: '🤫',
			color: 'bg-[var(--pastel-blue)]'
		},
		{
			id: 'funny',
			label: '유머러스',
			icon: '😄',
			color: 'bg-[var(--pastel-yellow)]'
		},
		{
			id: 'caring',
			label: '배려깊은',
			icon: '🤗',
			color: 'bg-[var(--pastel-green)]'
		},
		{
			id: 'creative',
			label: '창의적',
			icon: '🎨',
			color: 'bg-[var(--pastel-purple)]'
		},
		{
			id: 'logical',
			label: '논리적',
			icon: '🤔',
			color: 'bg-[var(--pastel-orange)]'
		}
	];

	const hobbyOptions = [
		{
			id: 'music',
			label: '음악',
			icon: Music,
			color: 'bg-[var(--pastel-pink)]'
		},
		{
			id: 'reading',
			label: '독서',
			icon: BookOpen,
			color: 'bg-[var(--pastel-blue)]'
		},
		{
			id: 'gaming',
			label: '게임',
			icon: Gamepad2,
			color: 'bg-[var(--pastel-yellow)]'
		},
		{
			id: 'sports',
			label: '운동',
			icon: Heart,
			color: 'bg-[var(--pastel-green)]'
		},
		{
			id: 'photography',
			label: '사진',
			icon: Camera,
			color: 'bg-[var(--pastel-purple)]'
		},
		{
			id: 'drawing',
			label: '그림',
			icon: PenTool,
			color: 'bg-[var(--pastel-orange)]'
		}
	];

	const mbtiOptions = [
		'INTJ',
		'INTP',
		'ENTJ',
		'ENTP',
		'INFJ',
		'INFP',
		'ENFJ',
		'ENFP',
		'ISTJ',
		'ISFJ',
		'ESTJ',
		'ESFJ',
		'ISTP',
		'ISFP',
		'ESTP',
		'ESFP'
	];

	const groupCategories = {
		'학년/반': [
			'1학년 1반',
			'1학년 2반',
			'1학년 3반',
			'1학년 4반',
			'1학년 5반',
			'2학년 1반',
			'2학년 2반',
			'2학년 3반',
			'2학년 4반',
			'2학년 5반',
			'3학년 1반',
			'3학년 2반',
			'3학년 3반',
			'3학년 4반',
			'3학년 5반'
		],
		'학과/전공': [
			'컴퓨터공학과',
			'전자공학과',
			'기계공학과',
			'화학공학과',
			'건축공학과',
			'경영학과',
			'경제학과',
			'심리학과',
			'영어영문학과',
			'국어국문학과',
			'디자인학과',
			'미술학과',
			'음악학과',
			'체육교육과',
			'간호학과'
		],
		'동아리/부서': [
			'축구부',
			'농구부',
			'야구부',
			'테니스부',
			'배드민턴부',
			'수영부',
			'밴드부',
			'댄스부',
			'연극부',
			'방송부',
			'사진부',
			'영화감상부',
			'미술부',
			'만화동아리',
			'문예창작부',
			'토론부',
			'영어회화부',
			'과학탐구부',
			'수학연구부',
			'컴퓨터동아리',
			'IT동아리',
			'창업동아리'
		],
		'취미/관심사': [
			'독서모임',
			'게임동아리',
			'보드게임모임',
			'K-POP팬클럽',
			'애니메이션동아리',
			'요리동아리',
			'카페탐방',
			'여행동아리',
			'등산동아리',
			'자전거동아리',
			'봉사동아리',
			'환경보호모임',
			'펜팔클럽',
			'언어교환모임',
			'스터디그룹'
		]
	};

	function handlePersonalityToggle(personalityId) {
		if (data.personality.includes(personalityId)) {
			data.personality = data.personality.filter((p) => p !== personalityId);
		} else {
			if (data.personality.length < 3) {
				data.personality = [...data.personality, personalityId];
			}
		}
	}

	function handleHobbyToggle(hobbyId) {
		if (data.hobbies.includes(hobbyId)) {
			data.hobbies = data.hobbies.filter((h) => h !== hobbyId);
		} else {
			if (data.hobbies.length < 3) {
				data.hobbies = [...data.hobbies, hobbyId];
			}
		}
	}

	function canProceed() {
		if (step === 1) return data.nickname.length >= 2;
		if (step === 2) return data.personality.length > 0;
		if (step === 3) return data.hobbies.length > 0;
		if (step === 4) return data.mbti !== '';
		if (step === 5) return data.group !== '';
		return false;
	}

	function getFilteredGroups() {
		const groups = groupCategories[selectedCategory] || [];
		if (!searchTerm) return groups;
		return groups.filter((group) => group.toLowerCase().includes(searchTerm.toLowerCase()));
	}

	function handleCustomGroupSubmit() {
		if (customGroup.trim()) {
			data.group = customGroup.trim();
			showCustomInput = false;
			customGroup = '';
		}
	}
</script>

<!-- 전체 화면 -->
<div
	class="flex min-h-screen flex-col bg-gradient-to-br from-[var(--pastel-pink)] via-[var(--pastel-blue)] to-[var(--pastel-purple)] p-4"
>
	<!-- Header -->
	<div class="mt-4 mb-6 text-center">
		<AmicoLogo size="lg" className="text-white justify-center" />
		<p class="mt-2 text-white/80">프로필을 설정하여 친구를 찾아보세요</p>
	</div>

	<!-- Progress Bar -->
	<div class="mx-auto mb-8 w-full max-w-md">
		<div class="mb-2 flex items-center justify-between">
			<span class="text-sm font-medium text-white">단계 {step}/5</span>
			<span class="text-sm text-white/70">{Math.round((step / 5) * 100)}%</span>
		</div>
		<div class="h-2 w-full rounded-full bg-white/20">
			<div
				class="h-2 rounded-full bg-white transition-all duration-300"
				style="width: {(step / 5) * 100}%"
			></div>
		</div>
	</div>

	<!-- Step Content -->
	<div class="flex flex-1 items-center justify-center">
		{#if error}
			<div class="fixed top-4 left-1/2 z-50 mx-4 w-full max-w-md -translate-x-1/2 transform">
				<div
					class="rounded-lg border-blue-200 bg-gradient-to-r from-blue-50 to-green-50 p-4 text-center text-blue-700"
				>
					{error}
					{#if error.includes('데모 모드') || error.includes('회원가입에 실패')}
						<div class="mt-3">
							<button
								class="rounded-md border-0 bg-gradient-to-r from-[var(--soft-green)] to-[var(--soft-blue)] px-4 py-2 text-white hover:opacity-90"
								on:click={() => {
									error = ''; /* onComplete(data) */
								}}
							>
								✨ 데모로 바로 시작하기 ✨
							</button>
						</div>
					{/if}
				</div>
			</div>
		{/if}
		<!-- 단계별 화면 -->
		{#if step === 1}
			<div class="mx-auto w-full max-w-md rounded-xl bg-white p-6 shadow-lg">
				<div class="mb-4 text-center">
					<div class="flex items-center justify-center gap-2 text-2xl">
						<span>👋</span>
						안녕하세요!
					</div>
					<p class="text-muted-foreground">닉네임을 입력해주세요</p>
				</div>
				<div class="space-y-4">
					<div class="space-y-2">
						<label for="nickname" class="block text-sm font-medium">닉네임</label>
						<input
							id="nickname"
							type="text"
							placeholder="2글자 이상 입력해주세요"
							class="w-full rounded-md border px-3 py-2"
							bind:value={data.nickname}
						/>
					</div>
				</div>
			</div>
		{:else if step === 2}
			<div class="mx-auto w-full max-w-md rounded-xl bg-white p-6 shadow-lg">
				<div class="mb-4 text-center">
					<div class="flex items-center justify-center gap-2 text-2xl">
						<span>😊</span>
						성격을 선택해주세요
					</div>
					<p class="text-muted-foreground">최대 3개까지 선택 가능해요</p>
				</div>
				<div class="grid grid-cols-2 gap-3">
					{#each personalityOptions as option}
						<button
							type="button"
							class={`rounded-lg border-2 p-4 transition-all ${data.personality.includes(option.id) ? 'border-[var(--soft-blue)] bg-[var(--pastel-blue)]' : 'border-gray-200 hover:border-gray-300'} ${option.color}`}
							on:click={() => handlePersonalityToggle(option.id)}
						>
							<div class="text-center">
								<div class="mb-1 text-2xl">{option.icon}</div>
								<div class="text-sm font-medium">{option.label}</div>
							</div>
						</button>
					{/each}
				</div>
				<div class="mt-4 text-center">
					<span class="inline-block rounded-full bg-gray-100 px-3 py-1 text-xs text-gray-700"
						>{data.personality.length}/3 선택됨</span
					>
				</div>
			</div>
		{:else if step === 3}
			<div class="mx-auto w-full max-w-md rounded-xl bg-white p-6 shadow-lg">
				<div class="mb-4 text-center">
					<div class="flex items-center justify-center gap-2 text-2xl">
						<span>🎯</span>
						취미를 선택해주세요
					</div>
					<p class="text-muted-foreground">최대 3개까지 선택 가능해요</p>
				</div>
				<div class="grid grid-cols-2 gap-3">
					{#each hobbyOptions as option}
						<button
							type="button"
							class={`rounded-lg border-2 p-4 transition-all ${data.hobbies.includes(option.id) ? 'border-[var(--soft-green)] bg-[var(--pastel-green)]' : 'border-gray-200 hover:border-gray-300'} ${option.color}`}
							on:click={() => handleHobbyToggle(option.id)}
						>
							<div class="text-center">
								<svelte:component this={option.icon} class="mx-auto mb-1 h-6 w-6" />
								<div class="text-sm font-medium">{option.label}</div>
							</div>
						</button>
					{/each}
				</div>
				<div class="mt-4 text-center">
					<span class="inline-block rounded-full bg-gray-100 px-3 py-1 text-xs text-gray-700"
						>{data.hobbies.length}/3 선택됨</span
					>
				</div>
			</div>
		{:else if step === 4}
			<div class="mx-auto w-full max-w-md rounded-xl bg-white p-6 shadow-lg">
				<div class="mb-4 text-center">
					<div class="flex items-center justify-center gap-2 text-2xl">
						<span>🧠</span>
						MBTI를 선택해주세요
					</div>
					<p class="text-muted-foreground">모르겠다면 나중에 변경할 수 있어요</p>
				</div>
				<div class="grid grid-cols-4 gap-2">
					{#each mbtiOptions as mbti}
						<button
							type="button"
							class={`rounded-lg border-2 p-3 text-sm font-medium transition-all ${data.mbti === mbti ? 'border-[var(--soft-purple)] bg-[var(--pastel-purple)]' : 'border-gray-200 hover:border-gray-300'}`}
							on:click={() => (data.mbti = mbti)}
						>
							{mbti}
						</button>
					{/each}
				</div>
			</div>
		{:else if step === 5}
			<div class="mx-auto w-full max-w-lg rounded-xl bg-white p-6 shadow-lg">
				<div class="mb-4 text-center">
					<div class="flex items-center justify-center gap-2 text-2xl">
						<span>🏫</span>
						소속을 선택해주세요
					</div>
					<p class="text-muted-foreground">같은 그룹의 친구들을 만날 수 있어요</p>
				</div>
				<div class="space-y-4">
					<!-- 검색 바 -->
					<div class="relative">
						<Search class="absolute top-3 left-3 h-4 w-4 text-gray-400" />
						<input
							type="text"
							placeholder="그룹 검색..."
							class="w-full rounded-md border px-3 py-2 pl-10"
							bind:value={searchTerm}
						/>
					</div>
					<!-- 카테고리 탭 -->
					<div class="flex flex-wrap gap-2">
						{#each Object.keys(groupCategories) as category}
							<button
								type="button"
								class={`rounded-full px-3 py-1 text-xs font-medium transition-all ${selectedCategory === category ? 'bg-[var(--soft-orange)] text-white shadow-md' : 'bg-gray-100 text-gray-700 hover:bg-gray-200'}`}
								on:click={() => {
									selectedCategory = category;
									searchTerm = '';
								}}
							>
								{category}
							</button>
						{/each}
					</div>
					<!-- 선택된 그룹 표시 -->
					{#if data.group}
						<div
							class="rounded-lg border-2 border-[var(--soft-orange)] bg-[var(--pastel-orange)] p-3"
						>
							<div class="flex items-center justify-between">
								<span class="text-sm font-medium text-orange-800">선택된 그룹: {data.group}</span>
								<button
									type="button"
									class="text-xs text-orange-600 hover:text-orange-800"
									on:click={() => (data.group = '')}>변경</button
								>
							</div>
						</div>
					{/if}
					<!-- 커스텀 그룹 입력 -->
					{#if showCustomInput}
						<div class="space-y-2">
							<div class="flex gap-2">
								<input
									type="text"
									placeholder="새 그룹 이름을 입력하세요"
									class="w-full rounded-md border px-3 py-2"
									bind:value={customGroup}
									on:keypress={(e) => e.key === 'Enter' && handleCustomGroupSubmit()}
								/>
								<button
									type="button"
									class="rounded-md bg-[var(--soft-green)] px-3 py-2 text-white hover:bg-[var(--soft-green)]/90"
									on:click={handleCustomGroupSubmit}>추가</button
								>
							</div>
							<button
								type="button"
								class="rounded-md bg-gray-100 px-2 py-1 text-xs"
								on:click={() => {
									showCustomInput = false;
									customGroup = '';
								}}>취소</button
							>
						</div>
					{:else}
						<button
							type="button"
							class="flex w-full items-center justify-center gap-2 rounded-md border-2 border-dashed px-3 py-2 hover:border-[var(--soft-green)] hover:bg-[var(--pastel-green)]"
							on:click={() => (showCustomInput = true)}
						>
							<Plus class="mr-2 h-4 w-4" />새 그룹 만들기
						</button>
					{/if}
					<!-- 그룹 목록 -->
					<div class="grid max-h-48 grid-cols-1 gap-2 overflow-y-auto rounded-lg border p-2">
						{#if getFilteredGroups().length > 0}
							{#each getFilteredGroups() as group}
								<button
									type="button"
									class={`rounded-lg border-2 p-3 text-left text-sm font-medium transition-all ${data.group === group ? 'border-[var(--soft-orange)] bg-[var(--pastel-orange)] text-orange-800' : 'border-gray-200 hover:border-gray-300 hover:bg-gray-50'}`}
									on:click={() => (data.group = group)}
								>
									{group}
								</button>
							{/each}
						{:else}
							<div class="py-4 text-center text-sm text-gray-500">
								{searchTerm ? '검색 결과가 없습니다' : '그룹이 없습니다'}
							</div>
						{/if}
					</div>
					<!-- 선택 안내 -->
					<div class="text-center">
						<p class="text-xs text-muted-foreground">
							원하는 그룹이 없다면 "새 그룹 만들기"로 직접 생성할 수 있어요!
						</p>
					</div>
				</div>
			</div>
		{/if}
	</div>

	<!-- Navigation -->
	<div class="mx-auto mt-8 w-full max-w-md">
		<div class="flex gap-3">
			{#if step > 1}
				<button
					type="button"
					class="flex-1 rounded-md border px-4 py-2"
					on:click={() => (step = step - 1)}>이전</button
				>
			{/if}
			<button
				type="button"
				class="flex flex-1 items-center justify-center gap-2 rounded-md bg-[var(--soft-blue)] px-4 py-2 text-white hover:bg-[var(--soft-blue)]/90"
				on:click={() => {
					if (canProceed() && !isLoading) step = step < 5 ? step + 1 : step;
				}}
				disabled={!canProceed() || isLoading}
			>
				{isLoading ? '처리 중...' : step === 5 ? '완료' : '다음'}
				{#if !isLoading}
					<ChevronRight class="ml-1 h-4 w-4" />
				{/if}
			</button>
		</div>
	</div>
</div>
