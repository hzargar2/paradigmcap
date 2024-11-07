<script setup lang="ts">

// const { data } = await useFetch('https://paradigmapi.pythonanywhere.com/api/clients')

definePageMeta({
    layout: 'dashboard'
})

</script>

<!--<div class="side-nav"> <div class="side-nav-sticky-container> INSERT CONTENT HERE </div> </div>-->

<!--.side-nav { position: relative; display: flex; flex-direction: column; width: 192px; min-width: 192px; }-->

<!--.side-nav-sticky-container { position: sticky; top: 0; width: auto; max-height: 100vh; overflow: auto; } -->

<template>
    <div class="flex flex-row h-full w-full relative z-[1]">

        <div class="min-w-fit w-[20%] flex h-screen sticky pt-[64px] top-0">

            <div class="w-full overflow-y-auto bg-slate-100 tracking-wider">
                <div class="px-8 pt-5 pb-3 backdrop-blur-md sticky top-0 flex items-center self-center text-lg font-medium whitespace-nowrap border-b border-opacity-70 border-gray-200">
                    <svg class="w-5 h-5 text-gray-500 transition duration-75 group-hover:text-gray-900" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 22 21">
                        <path d="M16.975 11H10V4.025a1 1 0 0 0-1.066-.998 8.5 8.5 0 1 0 9.039 9.039.999.999 0 0 0-1-1.066h.002Z"/>
                        <path d="M12.5 0c-.157 0-.311.01-.565.027A1 1 0 0 0 11 1.02V10h8.975a1 1 0 0 0 1-.935c.013-.188.028-.374.028-.565A8.51 8.51 0 0 0 12.5 0Z"/>
                    </svg>
                    <span class="flex ml-3 mb-0 my-auto align-text-bottom leading-4">Dashboard</span>
                </div>
                <ul class="px-3 py-2 space-y-2 font-medium pt-2">
                    <li>
                        <a href="/" class="flex items-center p-2 font-medium text-gray-900 rounded-lg hover:bg-slate-200 group">
                            <svg class="flex-shrink-0 w-5 h-5 text-gray-500 transition duration-75 group-hover:text-gray-900" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"> <path fill-rule="evenodd" clip-rule="evenodd" d="M11.9426 1.25H12.0574C14.3658 1.24999 16.1748 1.24998 17.5863 1.43975C19.031 1.63399 20.1711 2.03933 21.0659 2.93414C21.9607 3.82895 22.366 4.96897 22.5603 6.41371C22.75 7.82519 22.75 9.63423 22.75 11.9426V12.0574C22.75 14.3658 22.75 16.1748 22.5603 17.5863C22.366 19.031 21.9607 20.1711 21.0659 21.0659C20.1711 21.9607 19.031 22.366 17.5863 22.5603C16.1748 22.75 14.3658 22.75 12.0574 22.75H11.9426C9.63423 22.75 7.82519 22.75 6.41371 22.5603C4.96897 22.366 3.82895 21.9607 2.93414 21.0659C2.03933 20.1711 1.63399 19.031 1.43975 17.5863C1.24998 16.1748 1.24999 14.3658 1.25 12.0574V11.9426C1.24999 9.63423 1.24998 7.82519 1.43975 6.41371C1.63399 4.96897 2.03933 3.82895 2.93414 2.93414C3.82895 2.03933 4.96897 1.63399 6.41371 1.43975C7.82519 1.24998 9.63423 1.24999 11.9426 1.25ZM6.61358 2.92637C5.33517 3.09825 4.56445 3.42514 3.9948 3.9948C3.42514 4.56445 3.09825 5.33517 2.92637 6.61358C2.75159 7.91356 2.75 9.62178 2.75 12C2.75 14.3782 2.75159 16.0864 2.92637 17.3864C3.09825 18.6648 3.42514 19.4355 3.9948 20.0052C4.56445 20.5749 5.33517 20.9018 6.61358 21.0736C7.91356 21.2484 9.62178 21.25 12 21.25C14.3782 21.25 16.0864 21.2484 17.3864 21.0736C18.6648 20.9018 19.4355 20.5749 20.0052 20.0052C20.5749 19.4355 20.9018 18.6648 21.0736 17.3864C21.2484 16.0864 21.25 14.3782 21.25 12C21.25 9.62178 21.2484 7.91356 21.0736 6.61358C20.9018 5.33517 20.5749 4.56445 20.0052 3.9948C19.4355 3.42514 18.6648 3.09825 17.3864 2.92637C16.0864 2.75159 14.3782 2.75 12 2.75C9.62178 2.75 7.91356 2.75159 6.61358 2.92637ZM10.948 5.25H13.052C13.9505 5.24997 14.6997 5.24995 15.2945 5.32991C15.9223 5.41432 16.4891 5.59999 16.9445 6.05546C17.4 6.51093 17.5857 7.07773 17.6701 7.70552C17.7501 8.3003 17.75 9.04952 17.75 9.94801L17.75 10.25H19C19.4142 10.25 19.75 10.5858 19.75 11C19.75 11.4142 19.4142 11.75 19 11.75H5C4.58579 11.75 4.25 11.4142 4.25 11C4.25 10.5858 4.58579 10.25 5 10.25H6.25L6.25 9.94801C6.24997 9.04952 6.24995 8.3003 6.32991 7.70552C6.41432 7.07773 6.59999 6.51093 7.05546 6.05546C7.51093 5.59999 8.07773 5.41432 8.70552 5.32991C9.3003 5.24995 10.0495 5.24997 10.948 5.25ZM7.75 10.25H16.25V10C16.25 9.03599 16.2484 8.38843 16.1835 7.90539C16.1214 7.44393 16.0142 7.24643 15.8839 7.11612C15.7536 6.9858 15.5561 6.87858 15.0946 6.81654C14.6116 6.75159 13.964 6.75 13 6.75H11C10.036 6.75 9.38843 6.75159 8.90539 6.81654C8.44393 6.87858 8.24643 6.9858 8.11612 7.11612C7.9858 7.24643 7.87858 7.44393 7.81654 7.90539C7.75159 8.38843 7.75 9.03599 7.75 10V10.25ZM7.25 16C7.25 15.5858 7.58579 15.25 8 15.25H16C16.4142 15.25 16.75 15.5858 16.75 16C16.75 16.4142 16.4142 16.75 16 16.75H8C7.58579 16.75 7.25 16.4142 7.25 16Z" fill="#5e5c64"></path> </g></svg>
                            <span class="flex-1 ml-3 mb-0 my-auto align-text-bottom leading-4">Summary</span>
                        </a>
                    </li>
                    <li>
                        <a href="/clients" class="flex items-center font-medium p-2 text-gray-900 rounded-lg hover:bg-slate-200 group">
                            <svg class="flex-shrink-0 w-6 h-6 text-gray-500 transition duration-75 group-hover:text-gray-900" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" fill="#000000"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"><path d="M670.5 471.7c-7.1-3.1-14.2-5.9-21.4-8.5 49.8-40.3 81.6-101.8 81.6-170.6 0-121-98.4-219.4-219.4-219.4s-219.4 98.4-219.4 219.4c0 68.9 31.9 130.5 81.7 170.7C219.4 519.6 109 667.8 109 841.3h73.1c0-181.5 147.7-329.1 329.1-329.1 45.3 0 89.1 9 130.2 26.7l29.1-67.2zM511.3 146.3c80.7 0 146.3 65.6 146.3 146.3S592 438.9 511.3 438.9 365 373.2 365 292.6s65.6-146.3 146.3-146.3zM612.5 636.5c0 10.2 5.6 19.5 14.6 24.2l128 67.6c4 2.1 8.4 3.2 12.8 3.2s8.8-1.1 12.8-3.2l128-67.6c9-4.8 14.6-14.1 14.6-24.2s-5.6-19.5-14.6-24.2l-128-67.7c-8-4.2-17.6-4.2-25.6 0l-128 67.7c-9 4.7-14.6 14-14.6 24.2z m155.4-36.6l69.3 36.6-69.3 36.6-69.3-36.6 69.3-36.6z" fill="#5e5c64"></path><path d="M767.9 763.4l-147-77.7-25.6 48.5 172.6 91.2 171.9-90.8-25.6-48.5z" fill="#5e5c64"></path><path d="M767.9 851.4l-147-77.6-25.6 48.4 172.6 91.3 171.3-90.6-25.6-48.5z" fill="#5e5c64"></path></g></svg>
                            <span class="flex-1 ml-3 mb-0 my-auto align-text-bottom leading-4">Clients</span>
                        </a>
                    </li>
                </ul>
            </div>
        </div>

        <!--    content for page starts here-->
        <div class="flex flex-row overflow-x-hidden h-full mt-[64px] w-full p-6 xl:pr-12 2xl:pr-24">
            <slot/>
        </div>

    </div>
</template>

