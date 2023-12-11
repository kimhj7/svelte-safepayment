<script context="module">
    import { goto } from '$app/navigation';
</script>
<script>
    let username = '';
    let email = '';
    let password = '';
    let bankname = '';
    let accountNo = '';
    let message = '';

    async function registerUser() {
        const userData = {
            username,
            email,
            password,
            bankname,
            accountNo
        };

        try {
            const response = await fetch('http://118.44.159.249:1337/api/auth/local/register', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(userData),
            });

            if (response.ok) {
                // 회원 등록이 성공한 경우
                message = '회원 등록이 완료되었습니다.';
                goto('/login');

            } else {
                // 회원 등록이 실패한 경우
                const data = await response.json();
                message = data.message;
            }
        } catch (error) {
            console.error('회원 등록 중 오류 발생:', error);
            message = '회원 등록 중 오류가 발생했습니다.';
        }
    }

    function preventHyphen() {
        console.log("no hypen");
        this.value = this.value.replace(/[^0-9\.]/g,'');
    }
</script>
<style>

    .register-wrap {
        height: calc(100vh - 68px);
    }
    .register-wrap form {
        position: absolute;
        width: 100%;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }
    input#email {
        -webkit-ime-mode:disabled;
        -moz-ime-mode:disabled;
        -ms-ime-mode:disabled;
        ime-mode:disabled;
    }
</style>
<main>
    <div class="w-full flex justify-center align-center register-wrap">
        <form class="w-96 max-w-lg mx-auto border border-gray-200 rounded-lg p-6" on:submit={registerUser}>
            <div class="mb-5">
                <label for="username" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">이름</label>
                <input type="text" id="username" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="test@test.com" required bind:value={username} >
            </div>
            <div class="mb-5">
                <label for="email" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">이메일</label>
                <input type="email" id="email" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="test@test.com" required bind:value={email}>
            </div>
            <div class="mb-5">
                <label for="password" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">패스워드</label>
                <input type="password" id="password" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required bind:value={password}>
            </div>
            <div class="mb-5">
                <label for="bankname" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">은행명</label>
                <input type="text" id="bankname" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required bind:value={bankname}>
            </div>
            <div class="mb-5">
                <label for="accountNo" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">계좌번호</label>
                <input type="text" id="accountNo" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required bind:value={accountNo} on:keyup={preventHyphen}>
                <p class="mt-2 text-sm text-red-600 dark:text-red-500"><span class="font-medium">"-"를 제외한 숫자만 입력하세요.</span></p>
            </div>

            <button type="submit" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">회원가입</button>
        </form>
        {#if message}
            <p>{message}</p>
        {/if}
    </div>
</main>
