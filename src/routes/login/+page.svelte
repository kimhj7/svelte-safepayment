<script>
    import { onMount } from 'svelte';


    let identifier = '';
    let password = '';
    let loggedIn = false;
    let userData = null;

    async function login() {
        const response = await fetch('http://118.44.159.249:1337/api/auth/local', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ identifier, password })
        });
        if (response.ok) {
            // 로그인 성공
            const userData = await response.json();
            localStorage.setItem('user', JSON.stringify(userData)); // 사용자 정보 저장
            loggedIn = true; // 로그인 상태 업데이트
            window.location.reload();
        } else {
            // 로그인 실패
            loggedIn = false;
        }
    }
    function logout() {
        localStorage.removeItem('user'); // 사용자 정보 제거
        loggedIn = false; // 로그인 상태 업데이트
    }

    // 페이지 로드 시 로그인 상태 확인
    onMount(() => {
        const user = localStorage.getItem('user');
        if (user) {
            loggedIn = true;
            userData = JSON.parse(user);
        }
    });
</script>
<style>

    .login-wrap {
        height: calc(100vh - 68px);
    }
    .login-wrap form {
        position: absolute;
        width: 100%;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }
</style>
{#if loggedIn}
    <!-- 로그인 성공한 경우에 보여줄 내용 -->
    <section class="flex flex-wrap sm:justify-start sm:flex-nowrap z-50 w-full bg-gray-200 text-sm py-3 sm:py-0">
        <div class="relative max-w-[70rem] w-full mx-auto px-4 sm:px-6 lg:px-8 grid grid-cols-2 gap-4" aria-label="Global">

            <div class="col-left max-w-[100rem] py-10">
                <div class="w-lg p-6 mb-6 bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700">
                    <h5 class="mb-4 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">계좌잔액</h5>
                    <h3 class="mb-1 text-4xl font-bold text-gray-700 dark:text-gray-400">₩10,000,000</h3>
                    <p class="mb-6">출금가능</p>
                    <a href="/" class="inline-flex items-center px-3 py-2 text-sm font-medium text-center text-white bg-blue-700 rounded-lg hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
                        출금신청
                    </a>
                </div>
                <div class="w-lg p-6 mb-6 bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700">
                    <h5 class="mb-4 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">최근활동내역</h5>
                    <h5 class="text-xl">금액이 언제 입금되고 출금되는지 확인하세요.<br>최근 PayPal 활동내역을 여기에서 확인할 수 있습니다.</h5>
                </div>
            </div>
            <div class="col-right bg-slate-50">
                <div class="w-full justify-center">
                    <div class="flex justify-end px-4 pt-4">
                        <button id="dropdownButton" data-dropdown-toggle="dropdown" class="inline-block text-gray-500 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-700 focus:ring-4 focus:outline-none focus:ring-gray-200 dark:focus:ring-gray-700 rounded-lg text-sm p-1.5" type="button">
                            <span class="sr-only">Open dropdown</span>
                            <svg class="w-5 h-5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 16 3">
                                <path d="M2 0a1.5 1.5 0 1 1 0 3 1.5 1.5 0 0 1 0-3Zm6.041 0a1.5 1.5 0 1 1 0 3 1.5 1.5 0 0 1 0-3ZM14 0a1.5 1.5 0 1 1 0 3 1.5 1.5 0 0 1 0-3Z"/>
                            </svg>
                        </button>
                        <!-- Dropdown menu -->
                        <div id="dropdown" class="z-10 hidden text-base list-none bg-white divide-y divide-gray-100 rounded-lg shadow w-44 dark:bg-gray-700">
                            <ul class="py-2" aria-labelledby="dropdownButton">
                                <li>
                                    <a href="/" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-200 dark:hover:text-white">Edit</a>
                                </li>
                                <li>
                                    <a href="/" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-200 dark:hover:text-white">Export Data</a>
                                </li>
                                <li>
                                    <a href="/" class="block px-4 py-2 text-sm text-red-600 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-200 dark:hover:text-white">Delete</a>
                                </li>
                            </ul>
                        </div>
                    </div>
                    <div class="flex flex-col items-center pb-10">
                        <img class="w-24 h-24 mb-3 rounded-full shadow-lg" src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTCYWWkjwlLOmCKwbnYyDicbmmqEbRlUzFF3Qo13a6AUdPnuq4KfMjawDkaYMA_RQc-HEs&usqp=CAU" alt="Bonnie image"/>
                        <h5 class="mb-1 text-xl font-medium text-gray-900 dark:text-white">{userData.user.username}</h5>
                        <span class="text-sm text-gray-500 dark:text-gray-400">{userData.user.email}</span>
                        <div class="flex mt-4 md:mt-6">
                            <a href="/" class="inline-flex items-center px-4 py-2 text-sm font-medium text-center text-white bg-blue-700 rounded-lg hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">보내기</a>
                            <a href="/" class="inline-flex items-center px-4 py-2 text-sm font-medium text-center text-gray-900 bg-white border border-gray-300 rounded-lg hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-gray-200 dark:bg-gray-800 dark:text-white dark:border-gray-600 dark:hover:bg-gray-700 dark:hover:border-gray-700 dark:focus:ring-gray-700 ms-3">알림</a>
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </section>
{:else}
    <div class="w-full flex justify-center align-center login-wrap">
        <form class="w-96 max-w-lg mx-auto border border-gray-200 rounded-lg p-6" on:submit|preventDefault={login}>
            <div class="mb-5">
                <label for="email" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">이메일</label>
                <input type="email" id="email" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="test@test.com" required bind:value={identifier} >
            </div>
            <div class="mb-5">
                <label for="password" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">패스워드</label>
                <input type="password" id="password" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required bind:value={password}>
            </div>

            <button type="submit" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">로그인</button>
        </form>
    </div>

{/if}
