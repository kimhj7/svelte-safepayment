<script context="module">
    import { goto } from '$app/navigation';
</script>
<script>

    let userData = [];

    let now = new Date();

    async function fetchData() {
        try {
            const response = await fetch('http://118.44.159.249:1337/api/reg-users');
            userData = await response.json();
            userData = userData.data;

        } catch (error) {
            console.error('Error fetching data:', error);
        }
    }

    $: fetchData();

    let options = { hour12: false };

    function formatMyDate(value, locale = 'ko-KR') {
        return new Date(value).toLocaleString(locale, options);
    }



</script>
<div class="heading bg-cyan-700 flex justify-between items-center">
    <div class="breadcrum text-white px-10 py-4">
        <p>회원정보 > 회원관리</p>
    </div>
    <div class="heading-util">
        <button type="button" class="text-gray-900 bg-white border border-gray-300 focus:outline-none hover:bg-gray-100 focus:ring-4 focus:ring-gray-200 font-medium rounded-lg text-sm px-5 py-2.5 me-2 dark:bg-gray-800 dark:text-white dark:border-gray-600 dark:hover:bg-gray-700 dark:hover:border-gray-600 dark:focus:ring-gray-700" on:click={() => goto('/userinfo/registerUser')}>회원등록</button>
    </div>
</div>
<div class="container mx-auto mt-4">

    {#if userData.length > 0}
        <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
            <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
            <tr class="bg-slate-100 border-b dark:bg-gray-800 dark:border-gray-700 text-center">
                <th scope="col" class="px-6 py-3">NO</th>
                <th scope="col" class="px-6 py-3">아이디</th>
                <th scope="col" class="px-6 py-3">휴대폰</th>
                <th scope="col" class="px-6 py-3">실명</th>
                <th scope="col" class="px-6 py-3">출금은행</th>
                <th scope="col" class="px-6 py-3">출금계좌번호</th>
                <th scope="col" class="px-6 py-3">입금은행</th>
                <th scope="col" class="px-6 py-3">입금계좌번호</th>
                <th scope="col" class="px-6 py-3">가입일자</th>
            </tr>
            </thead>
            <tbody>
            {#each userData as item}
                <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 text-center">
                    <td class="px-6 py-4">{item.id}</td>
                    <td class="px-6 py-4">{item.attributes.userid}</td>
                    <td class="px-6 py-4">{item.attributes.phone}</td>
                    <td class="px-6 py-4">{item.attributes.username}</td>
                    <td class="px-6 py-4">{item.attributes.w_bank}</td>
                    <td class="px-6 py-4">{item.attributes.w_account}</td>
                    <td class="px-6 py-4">{item.attributes.d_bank}</td>
                    <td class="px-6 py-4">{item.attributes.d_account}</td>
                    <td class="px-6 py-4">{formatMyDate(item.attributes.regdate)}</td>
                </tr>

            {/each}
            </tbody>
        </table>
    {:else}
        <p>Loading...</p>
    {/if}
</div>
