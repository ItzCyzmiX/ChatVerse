<script>
    import { goto } from "$app/navigation";
  
    let error = $state("");
    let search = $state("");
    // Mock data for chat bots
    let data = $props();
    if (!data.data?.user) {
        error = "You are not logged in";
    }
    let myBots = data.data?.user?.createdBots || [];
    let username = data.data?.user?.username;
    let allBots = data.data?.randomBots || [];
    let randomBots = allBots.filter((bot) => !myBots.includes(bot.name));
    randomBots = randomBots.slice(0, 4);
</script>

{#if error}
    <div
        class="fixed inset-0 bg-black/50 backdrop-blur-sm z-50 flex items-center justify-center"
    >
        <div
            class="bg-zinc-900 border-2 border-white/20 rounded-xl p-8 max-w-md w-full mx-4"
        >
            <div class="text-center">
                <h2 class="text-3xl text-red-500 font-bold mb-4">404</h2>
                <p class="text-zinc-400 mb-6">{error}</p>
                <button
                    class="bg-white text-black px-6 py-2 rounded-lg font-medium hover:bg-zinc-200 transition-colors"
                    onclick={() => goto("/login")}
                >
                    Go to Login
                </button>
            </div>
        </div>
    </div>
{:else}
    <div class="min-h-screen text-white p-8">
        <div class="fixed inset-0 -z-[11] h-full w-full bg-black">
            <div
                class="absolute bottom-0 left-0 right-0 top-0 bg-[linear-gradient(to_right,#4f4f4f2e_1px,transparent_1px)] bg-[size:40px_40px]"
            ></div>
            <div
                class="absolute bottom-0 left-0 right-0 top-0 bg-[linear-gradient(to_bottom,#4f4f4f2e_1px,transparent_1px)] bg-[size:40px_40px]"
            ></div>
        </div>
        <div class="absolute top-8 left-8">
            <a
                href="/new"
                class="flex items-center gap-3 bg-zinc-800/30 border-2 border-white/20 rounded-xl px-6 py-3 text-white hover:bg-zinc-900 transition-all duration-200"
            >
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="h-6 w-6"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M12 4v16m8-8H4"
                    />
                </svg>
                <span class="font-medium">Create New Chatbot</span>
            </a>
        </div>
        <div class="absolute top-8 right-8">
            <a
                href={`/users/${username}`}
                class="flex items-center space-x-2 bg-zinc-800/30 border-2 border-white/20 rounded-xl px-4 py-2 text-white hover:bg-zinc-900 transition-all duration-200 w-fit"
            >
                <div
                    class="w-8 h-8 rounded-full bg-white/20 flex items-center justify-center"
                >
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        class="h-5 w-5 text-white"
                        viewBox="0 0 20 20"
                        fill="currentColor"
                    >
                        <path
                            fill-rule="evenodd"
                            d="M10 9a3 3 0 100-6 3 3 0 000 6zm-7 9a7 7 0 1114 0H3z"
                            clip-rule="evenodd"
                        />
                    </svg>
                </div>
                <span class="font-medium">Profile</span>
            </a>
        </div>

        <div class="mb-14">
            <div class="flex justify-center">
                <div class="relative">

                    <input
                        oninput={(e) => {
                            search = e.target.value;
                        }}
                        type="text"
                        placeholder="Search chatbots..."
                        class="w-96 bg-zinc-800/30 border-2 border-white/20 rounded-xl px-6 py-3 text-white placeholder-zinc-500 focus:outline-none focus:border-white/40 shadow-md shadow-white/10"
                    />
                    <svg
                        class="w-5 h-5 text-zinc-500 absolute right-4 top-1/2 -translate-y-1/2"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                    >
                        <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            stroke-width="2"
                            d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
                        />
                    </svg>
                    {#if search}
                        <div class="absolute top-full left-0 right-0 mt-2 bg-zinc-800/95 border-2 border-white/20 rounded-xl overflow-hidden shadow-lg">
                            {#each allBots as bot}
                                {#if bot.name.toLowerCase().includes(search.toLowerCase())}
                                    <a href={`/chat/${bot.name}`} class="flex items-center gap-3 p-4 hover:bg-zinc-700/50 transition-colors">
                                        <div class="w-8 h-8 rounded-full bg-white/10 flex items-center justify-center">
                                            <span class="text-white font-medium">{bot.name[0].toUpperCase()}</span>
                                        </div>
                                        <div>
                                            <p class="text-white font-medium">{bot.name}</p>
                                            <p class="text-zinc-400 text-sm">by {bot.creator}</p>
                                        </div>
                                    </a>
                                {/if}
                            {/each}
                        </div>
                    {/if}
                </div>
            </div>
        </div>
        <div class="max-w-7xl mx-auto">
            <section class="mb-12">
                <h2 class="text-3xl font-bold text-white mb-6">My Chatbots</h2>
                <div
                    class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6"
                >
                    {#each myBots as bot}
                        <div
                            class="bg-zinc-800/30 rounded-xl p-6 transition-all duration-200 hover:bg-zinc-900/30 cursor-pointer border-2 border-white/20"
                        >
                            <h3 class="text-2xl font-bold text-white mb-2">
                                {bot.name}
                            </h3>
                            <p class="text-zinc-400 text-sm">
                                {bot.description}
                            </p>
                            <a
                                href={`/chat/${bot.name}`}
                                class="text-center bg-white font-bold text-black rounded-xl p-4 cursor-pointer w-full h-fit mt-8 hover:transform hover:-translate-y-1 hover:shadow-lg hover:shadow-white/20 transition-all duration-200 block"
                            >
                                chat with <span
                                    class="underline decoration-2 underline-offset-4"
                                    >{bot.name}</span
                                >
                            </a>
                        </div>
                    {/each}
                </div>
            </section>

            <div class="h-px bg-zinc-700 my-8"></div>

            <section class="mb-12">
                <h2 class="text-3xl font-bold text-white mb-6">Random</h2>
                <div
                    class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6"
                >
                    {#each randomBots as bot}
                        <div
                            class="bg-zinc-800/30 rounded-xl p-6 transition-all duration-200 hover:bg-zinc-900/30 cursor-pointer border-2 border-white/20"
                        >
                            <h3 class="text-2xl font-bold text-white mb-2">
                                {bot.name}<span
                                    class="ml-2 text-zinc-400 text-sm font-medium"
                                >
                                    by {bot.creator}
                                </span>
                            </h3>
                            <p class="text-zinc-400 text-sm">
                                {bot.description}
                            </p>

                            <a
                                href={`/chat/${bot.name}`}
                                class="text-center bg-white font-bold text-black rounded-xl p-4 cursor-pointer w-full h-fit mt-8 hover:transform hover:-translate-y-1 hover:shadow-lg hover:shadow-white/20 transition-all duration-200 block"
                            >
                                chat with <span
                                    class="underline decoration-2 underline-offset-4"
                                    >{bot.name}</span
                                >
                            </a>
                        </div>
                    {/each}
                </div>
            </section>
        </div>
    </div>
{/if}
