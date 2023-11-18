<script setup lang="ts">

    import { ref, shallowRef } from 'vue';
    import NavRoute from './NavRoute';

    const sideIsOpen = ref(false);

    interface Nav {
        icon: string,
        label: string,
        action: Function,
        class?: string
    }

    const palette = ref<{[key : string] : string }>(
        {
            'container-bg'                  : '#eeeff2',
            'body-bg'                       : '#f7f7f7',
            'header-sidebar'                : '#e5e8f1',
            'item-sidebar'                  : '#e2e7f0',
            'item-icon'                     : '#dae0eb',
            'icon-color'                    : '#0069e4',
            'item-border'                   : '#b4b6b9',
        }
    );    

    const icons : {[key : string] : string } = {
        'bars' : `<svg xmlns="http://www.w3.org/2000/svg" height="28" viewBox="0 0 448 512"><path fill="${palette.value['icon-color']}" d="M0 88c0-4.4 3.6-8 8-8H440c4.4 0 8 3.6 8 8s-3.6 8-8 8H8c-4.4 0-8-3.6-8-8zM0 248c0-4.4 3.6-8 8-8H440c4.4 0 8 3.6 8 8s-3.6 8-8 8H8c-4.4 0-8-3.6-8-8zM448 408c0 4.4-3.6 8-8 8H8c-4.4 0-8-3.6-8-8s3.6-8 8-8H440c4.4 0 8 3.6 8 8z"/></svg>`,
        'close' : `<svg xmlns="http://www.w3.org/2000/svg" height="28" viewBox="0 0 384 512"><path fill="${palette.value['icon-color']}" d="M338.1 413.4c3.1 3.1 8.2 3.1 11.3 0s3.1-8.2 0-11.3L203.3 256 349.4 109.9c3.1-3.1 3.1-8.2 0-11.3s-8.2-3.1-11.3 0L192 244.7 45.9 98.6c-3.1-3.1-8.2-3.1-11.3 0s-3.1 8.2 0 11.3L180.7 256 34.6 402.1c-3.1 3.1-3.1 8.2 0 11.3s8.2 3.1 11.3 0L192 267.3 338.1 413.4z"/></svg>`,
        'folder' : `<svg xmlns="http://www.w3.org/2000/svg" width="20" viewBox="0 0 512 512"><path fill="${palette.value['icon-color']}" d="M64 464H448c26.5 0 48-21.5 48-48V160c0-26.5-21.5-48-48-48H301.3c-12.7 0-24.9-5.1-33.9-14.1L231.4 62.1c-9-9-21.2-14.1-33.9-14.1H64C37.5 48 16 69.5 16 96V416c0 26.5 21.5 48 48 48zm384 16H64c-35.3 0-64-28.7-64-64V96C0 60.7 28.7 32 64 32H197.5c17 0 33.3 6.7 45.3 18.7l35.9 35.9c6 6 14.1 9.4 22.6 9.4H448c35.3 0 64 28.7 64 64V416c0 35.3-28.7 64-64 64z"/></svg>`,
        'dot' : `<svg xmlns="http://www.w3.org/2000/svg" width="10" viewBox="0 0 512 512"><defs><style>.fa-secondary{opacity:.4}</style></defs><path fill="${palette.value['icon-color']}" d="M256 160a96 96 0 1 0 0 192 96 96 0 1 0 0-192z"/><path class="fa-secondary" fill="${palette.value['icon-color']}" d="M256 512A256 256 0 1 0 256 0a256 256 0 1 0 0 512zm0-352a96 96 0 1 1 0 192 96 96 0 1 1 0-192z"/></svg>`,
        'left' : `<svg xmlns="http://www.w3.org/2000/svg" width="10" viewBox="0 0 256 512"><path fill="${palette.value['icon-color']}" d="M22.6 271.4c-4.2-4-6.6-9.6-6.6-15.4s2.4-11.4 6.6-15.4L154.7 115.5c2.3-2.2 5.4-3.5 8.7-3.5c7 0 12.6 5.6 12.6 12.6l0 262.8c0 7-5.6 12.6-12.6 12.6c-3.2 0-6.3-1.2-8.7-3.5L22.6 271.4zM11.6 283L143.7 408.2c5.3 5 12.4 7.8 19.7 7.8c15.8 0 28.6-12.8 28.6-28.6l0-262.8c0-15.8-12.8-28.6-28.6-28.6c-7.3 0-14.4 2.8-19.7 7.8L11.6 229C4.2 236 0 245.8 0 256s4.2 20 11.6 27z"/></svg>`
    }

    const menuArray : {[key : string] : Nav[] } = {
        'STARTUP' : [
            {
                icon: icons['folder'],
                label: 'Languages',
                action: () => {
                    NavSchema.value = menuArray['Languages-sub'];
                }
            },
            {
                icon: icons['folder'],
                label: 'Frameworks',
                action: () => {
                    NavSchema.value = menuArray['Frameworks-sub'];
                }
            },
        ],

        'Languages-sub' : [
            {
                icon: icons['left'],
                label: 'Languages',
                action: () => {
                    NavSchema.value = menuArray['STARTUP']
                },
                class: 'my-custom-item'
            },
            {
                icon: icons['dot'],
                label: 'PHP',
                action: () => {
                    sideIsOpen.value = false;
                    itemSelected.value = 'PHP';
                    component.value = NavRoute.components.PHPComponent;
                }
            },
            {
                icon: icons['dot'],
                label: 'Javascript',
                action: () => {
                    sideIsOpen.value = false;
                    itemSelected.value = 'Javascript';
                    component.value = NavRoute.components.JSComponent;
                }
            },
        ],

        'Frameworks-sub' : [
            {
                icon: icons['left'],
                label: 'Frameworks',
                action: () => {
                    NavSchema.value = menuArray['STARTUP']
                },
                class: 'my-custom-item'
            },
            {
                icon: icons['dot'],
                label: 'Symfony',
                action: () => {
                    sideIsOpen.value = false;
                    itemSelected.value = 'Symfony';
                    component.value = NavRoute.components.SMFComponent;
                }
            },
            {
                icon: icons['dot'],
                label: 'Vue',
                action: () => {
                    sideIsOpen.value = false;
                    itemSelected.value = 'Vue';
                    component.value = NavRoute.components.VUEComponent;
                }
            },
        ],

    }

    const navMenuArray : {[key : string] : Nav[] } = {
        'NAVBAR' : [
            {
                icon: icons['dot'],
                label: 'PHP',
                action: () => {
                    NavSchema.value = menuArray['Languages-sub'];
                    itemSelected.value = 'PHP';
                    component.value = NavRoute.components.PHPComponent;
                }
            },
            {
                icon: icons['dot'],
                label: 'Symfony',
                action: () => {
                    NavSchema.value = menuArray['Frameworks-sub'];
                    itemSelected.value = 'Symfony';
                    component.value = NavRoute.components.SMFComponent;
                }
            },
            {
                icon: icons['dot'],
                label: 'Vue',
                action: () => {
                    NavSchema.value = menuArray['Frameworks-sub'];
                    itemSelected.value = 'Vue';
                    component.value = NavRoute.components.VUEComponent;
                }
            },
        ],
    }

    const NavSchema = ref<Nav[]>(menuArray['STARTUP']);
    const NavItemSchema = ref<Nav[]>(navMenuArray['NAVBAR']);

    const itemSelected = ref('');

    const component = shallowRef(NavRoute.components.WComponent)

</script>

<template>

    <div class="wrap-container">
        <div class="sidebar"
            v-if="sideIsOpen"
        >
            <div class="sidebar-header">
                <div class="sidebar-header-heading flex-center-start">
                    Menu
                </div>
                <div class="sidebar-header-icon flex-center pointer"
                    @click="sideIsOpen = false"
                    v-html="icons['close']"
                >
                </div>
            </div>

            <div class="side-nav-item animateToRight pointer"
                v-for="nav in NavSchema" @click="nav.action()"
                :class="nav.class ? nav.class : itemSelected === nav.label ? 'nav_item-selected' : null"
            >
                <div class="side-nav-item-icon flex-center"
                    v-html="nav.icon"
                >
                </div>
                <div class="side-nav-item-label flex-center-start">
                    {{ nav.label }}
                </div>
            </div>
        </div>
        <div class="container">
            <div class="container-header">
                <div class="brand-toggle-container"
                    v-if="!sideIsOpen"
                >
                    <div class="icon-toggle-sidebar flex-center pointer"
                        @click="sideIsOpen = true"
                        v-html="icons['bars']"
                    >
                    </div>
                    <div class="nav-brand flex-center-start">
                        BRAND
                    </div>
                </div>
                <div class="nav">
                    <div class="nav-item pointer"
                        v-for="nav in NavItemSchema" @click="nav.action()"
                        :class="nav.class ? nav.class : itemSelected === nav.label ? 'navbar_item-selected' : null"
                    >
                        <div class="nav-item-icon flex-center"
                            v-html="nav.icon"
                        >
                        </div>
                        <div class="nav-item-label flex-center-start">
                            {{ nav.label }}
                        </div>
                    </div>
                </div>
            </div>

            <div class="container-body">
            
                
                <component />


            </div>

            <div class="nav-mobile">
                <div class="nav-item pointer"
                    v-for="nav in NavItemSchema" @click="nav.action()"
                    :class="nav.class ? nav.class : itemSelected === nav.label ? 'navbar_item-selected' : null"
                >
                    <div class="nav-item-icon flex-center"
                        v-html="nav.icon"
                    >
                    </div>
                    <div class="nav-item-label flex-center-start">
                        {{ nav.label }}
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>

<style scoped>

    .wrap-container
    {
        display: flex;
        width: 100vw;
        max-width: 1400px;
        height: 100vh;
        margin-left: auto;
        margin-right: auto;
        overflow: hidden;
        background: v-bind('palette['container-bg']');
    }

    .sidebar
    {
        width: 280px;
        height: 100vh;
    }

    .container
    {
        width: 100%;
        height: 100vh;
    }

    .container-header
    {
        width: 100%;
        height: 50px;
        display: flex;
    }

    .brand-toggle-container
    {
        width: 280px;
        height: 50px;
        display: flex;
    }

    .icon-toggle-sidebar
    {
        width: 50px;
        height: 50px;
    }

    .nav-brand
    {
        width: 100%;
        height: 50px;
    }

    .nav
    {
        width: 100%;
        height: 50px;
        display: flex;
        justify-content: end;
    }

    .nav-item
    {
        width: 130px;
        height: 50px;
        display: flex;
        align-items: center;
    }

    .nav-item-icon 
    {
        width: 30px;
        height: 30px;
        border-left: solid 0.5px v-bind('palette['item-border']');
    }

    .nav-item-label 
    {
        width: 100px;
        height: 50px;
    }

    .container-body
    {
        width: 100%;
        height: calc(100vh - 50px);
        background: v-bind('palette['body-bg']');
        padding: 8px;
        overflow: scroll;
    }

    .flex-center
    {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .flex-center-start
    {
        display: flex;
        justify-content: start;
        align-items: center;
        padding-left: 5px;
    }

    .sidebar-header{
        width: 280px;
        height: 50px;
        display: flex;
        background: v-bind('palette['header-sidebar']');
        color: v-bind('palette['icon-color']');
    }

    .sidebar-header-heading
    {
        width: 250px;
        height: 50px;
    }

    .sidebar-header-icon
    {
        width: 30px;
        height: 50px;
    }

    .side-nav-item
    {
        display: flex;
        width: 280px;
        height: 30px;
        margin-bottom: 1px;
        background: v-bind('palette['item-sidebar']');
    }

    .side-nav-item-icon
    {
        width: 30px;
        height: 30px;
        background: v-bind('palette['item-icon']');
    }

    .side-nav-item-label
    {
        width: 250px;
        height: 30px;
    }

    .pointer:hover
    {
        cursor: pointer;
    }

    .animateToRight
    {
        animation: sideToRight 0.3s linear;        
    }

    .nav_item-selected
    {
        background: v-bind('palette['item-icon']');
        color: v-bind('palette['icon-color']');
    }

    .navbar_item-selected > .nav-item-icon
    {
        background: v-bind('palette['item-icon']');
        border: none;
        border-radius: 5px;
    }

    .navbar_item-selected > .nav-item-label
    {
        color: v-bind('palette['icon-color']');
    }

    .side-nav-item:hover
    {
        background: v-bind('palette['item-icon']');
    }

    @keyframes sideToRight {
        0%{
            transform: translateX(-100%);
            opacity: 0;
        }
        100%{
            transform: translateX(0%);
            opacity: 1;
        }
    }




    @media (max-width : 700px) {

        .container-body
        {
            width: 100%;
            height: calc(100vh - 100px);
        }        

        .nav
        {
            display:none;
        }        
        .nav-mobile
        {
            width: 100%;
            height: 50px;
            display: flex;
            justify-content: center;
        }

        .nav-item
        {
            height: 50px;
            display: flex;
            align-items: center;
        }

        .nav-item-icon 
        {
            width: 30px;
            height: 30px;
            border-left: solid 0.5px v-bind('palette['item-border']');
        }

        .nav-item-label 
        {
            height: 50px;
        }        
    }

    .my-custom-item
    {
        border-bottom: solid 1px v-bind('palette['item-border']');
        background: v-bind('palette['container-bg']');
        color: v-bind('palette['icon-color']');
    }

    .my-custom-item > .side-nav-item-icon
    {
        background: none;
    }

</style>