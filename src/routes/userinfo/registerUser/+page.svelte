<script context="module">
    import { goto } from '$app/navigation';
</script>
<script>
    let userid = '';
    let phone = '';
    let username = '';
    let w_bank = '';
    let w_account = '';
    let d_bank = '';
    let d_account = '';
    let regdate = new Date();
    let message = '';

    async function registerUser() {
        const userData = {
            userid,
            phone,
            username,
            w_bank,
            w_account,
            d_bank,
            d_account,
            regdate
        };

        try {
            const response = await fetch('http://118.44.159.249:1337/api/reg-users', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({data:userData}),
            });

            if (response.ok) {
                // 회원 등록이 성공한 경우
                message = '회원 등록이 완료되었습니다.';
                goto('/userinfo');

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

</style>
<main>
    <div class="w-full flex justify-center align-center register-wrap">
        <form class="w-96 max-w-lg mx-auto border border-gray-200 rounded-lg p-6" on:submit={registerUser}>
            <div class="mb-5">
                <label for="userid" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">사용자ID</label>
                <input type="text" id="userid" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="아이디" required bind:value={userid} >
            </div>
            <div class="mb-5">
                <label for="username" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">이름</label>
                <input type="text" id="username" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="이름" required bind:value={username}>
            </div>
            <div class="mb-5">
                <label for="phone" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">휴대폰번호</label>
                <input type="text" id="phone" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required bind:value={phone}>
            </div>
            <div class="mb-5">
                <label for="w_bank" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">출금은행명</label>
                <input type="text" id="w_bank" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required bind:value={w_bank}>
            </div>
            <div class="mb-5">
                <label for="w_account" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">출금계좌번호</label>
                <input type="text" id="w_account" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required bind:value={w_account} on:keyup={preventHyphen}>
                <p class="mt-2 text-sm text-red-600 dark:text-red-500"><span class="font-medium">"-"를 제외한 숫자만 입력하세요.</span></p>
            </div>
            <div class="mb-5">
                <label for="d_bank" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">입금은행명</label>
                <input type="text" id="d_bank" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required bind:value={d_bank}>
            </div>
            <div class="mb-5">
                <label for="d_account" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">입금계좌번호</label>
                <input type="text" id="d_account" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required bind:value={d_account} on:keyup={preventHyphen}>
                <p class="mt-2 text-sm text-red-600 dark:text-red-500"><span class="font-medium">"-"를 제외한 숫자만 입력하세요.</span></p>
            </div>

            <button type="submit" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">회원가입</button>
        </form>
        {#if message}
            <p>{message}</p>
        {/if}
    </div>
</main>
