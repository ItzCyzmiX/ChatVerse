<script>
    import { goto } from '$app/navigation'
    let data = $props()
    
    const userData = {
        name: data.data?.user?.username || '',
        profilePicture: "https://api.dicebear.com/7.x/avataaars/svg?seed=" + data.data?.user?.username || '',
        createdBots: data.data?.user?.createdBots || []
    };
        async function logout() {
        let res = await fetch('/users/' + userData.name)
        if (res.status === 200) {
            goto('/login');
        }
    }
</script>
<div class="min-h-screen text-white p-4 sm:p-8">
    <div class="fixed inset-0 -z-[11] h-full w-full bg-black">
        <div
            class="absolute bottom-0 left-0 right-0 top-0 bg-[linear-gradient(to_right,#4f4f4f2e_1px,transparent_1px)] bg-[size:40px_40px]"
        ></div>
        <div
            class="absolute bottom-0 left-0 right-0 top-0 bg-[linear-gradient(to_bottom,#4f4f4f2e_1px,transparent_1px)] bg-[size:40px_40px]"
        ></div>
    </div>
    <div class="max-w-7xl mx-auto">
        <div class="mb-8 sm:mb-12 bg-zinc-800/30 rounded-xl p-4 sm:p-8 border-2 border-white/20">
            <div class="flex flex-col sm:flex-row items-center gap-4 sm:gap-8">
                <img 
                    src={userData.profilePicture} 
                    alt={userData.name}
                    class="w-24 h-24 sm:w-32 sm:h-32 rounded-full border-4 border-white/20"
                />
                <div class="flex-grow text-center sm:text-left">
                    <h1 class="text-2xl sm:text-4xl font-bold mb-2">{userData.name}</h1>
                    <p class="text-zinc-400">@{userData.name}</p>
                </div>
                <button 
                    class="w-full sm:w-auto bg-red-500/20 text-red-500 px-6 py-2 rounded-lg hover:bg-red-500/30 transition-colors"
                    onclick={logout}
                >
                    Log Out
                </button>
            </div>
        </div>

        <section class="mb-8 sm:mb-12">
            <h2 class="text-2xl sm:text-3xl font-bold mb-4 sm:mb-6">Created Chatbots</h2>
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 sm:gap-6">
                {#if userData.createdBots.length === 0}
                    <p class="text-zinc-400">You havent created any chatbots yet</p>
                {:else}
                    {#each userData.createdBots as bot}
                    <div class="bg-zinc-800/30 rounded-xl p-4 sm:p-6 transition-all duration-200 hover:transform hover:-translate-y-1 hover:bg-zinc-900 cursor-pointer border-2 border-white/20">
                        <h3 class="text-lg sm:text-xl font-bold mb-2">{bot.name}</h3>
                        <p class="text-zinc-400 text-xs sm:text-sm">{bot.title}</p>
                        <p class="text-zinc-500 text-xs mt-4">
                            Created: {new Date(bot.createdAt).toLocaleDateString()}
                        </p>
                    </div>
                    {/each}
                {/if}
            </div>
        </section>
    </div>
</div>