<script setup lang="ts">

    import { ref } from 'vue';
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
            'icon-color'                    : '#80c6db',
            'item-border'                   : '#b4b6b9',
        }
    );    

    const svgCode = `
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
            <circle cx="50" cy="50" r="20" stroke="white" stroke-width="3" fill="${palette.value['icon-color']}" />
        </svg>
    `;

    const menuArray : {[key : string] : Nav[] } = {
        'STARTUP' : [
            {
                icon: svgCode,
                label: 'Languages',
                action: () => {
                    NavSchema.value = menuArray['Languages-sub'];
                }
            },
            {
                icon: svgCode,
                label: 'Frameworks',
                action: () => {
                    NavSchema.value = menuArray['Frameworks-sub'];
                }
            },
        ],

        'Languages-sub' : [
            {
                icon: svgCode,
                label: '',
                action: () => {
                    NavSchema.value = menuArray['STARTUP']
                },
                class: 'my-custom-item'
            },
            {
                icon: svgCode,
                label: 'PHP',
                action: () => {
                    sideIsOpen.value = false;
                    itemSelected.value = 'PHP';
                    component.value = NavRoute.components.PHPComponent;
                }
            },
            {
                icon: svgCode,
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
                icon: svgCode,
                label: '',
                action: () => {
                    NavSchema.value = menuArray['STARTUP']
                },
                class: 'my-custom-item'
            },
            {
                icon: svgCode,
                label: 'Symfony',
                action: () => {
                    sideIsOpen.value = false;
                    itemSelected.value = 'Symfony';
                    component.value = NavRoute.components.SMFComponent;
                }
            },
            {
                icon: svgCode,
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
                icon: svgCode,
                label: 'PHP',
                action: () => {
                    NavSchema.value = menuArray['Languages-sub'];
                    itemSelected.value = 'PHP';
                    component.value = NavRoute.components.PHPComponent;
                }
            },
            {
                icon: svgCode,
                label: 'Symfony',
                action: () => {
                    NavSchema.value = menuArray['Frameworks-sub'];
                    itemSelected.value = 'Symfony';
                    component.value = NavRoute.components.SMFComponent;
                }
            },
            {
                icon: svgCode,
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

    const component = ref(NavRoute.components.WComponent)

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
                    v-html="svgCode"
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
                <div class="side-nav-item-label flex-center">
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
                        v-html="svgCode"
                    >
                    </div>
                    <div class="nav-brand flex-center-start">
                        BRAND
                    </div>
                </div>
                <div class="nav">
                    <div class="nav-item pointer"
                        v-for="nav in NavItemSchema" @click="nav.action()"
                        :class="nav.class ? nav.class : itemSelected === nav.label ? 'nav_item-selected' : null"
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
                    :class="nav.class ? nav.class : itemSelected === nav.label ? 'nav_item-selected' : null"
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

</style>