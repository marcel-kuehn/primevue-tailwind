<template>
    <DocSectionText v-bind="$attrs">
        <p>ContextMenu offers item customization with the <i>item</i> template that receives the menuitem instance from the model as a parameter.</p>
    </DocSectionText>
    <div class="card flex md:justify-center">
        <ul class="m-0 p-0 list-none border border-surface-200 dark:border-surface-700 rounded p-3 flex flex-col gap-2 w-full md:w-[30rem]">
            <li
                v-for="product in products"
                :key="product.id"
                :class="[
                    'p-2 hover:bg-primary-50 dark:hover:bg-primary-400/30 rounded border transition-all duration-200',
                    { 'border-transparent': selectedId !== product.id },
                    { 'border-primary-500 dark:border-primary-400': selectedId === product.id }
                ]"
                @contextmenu="onRightClick($event, product.id)"
            >
                <div class="flex flex-wrap p-2 items-center gap-3">
                    <img class="w-[4rem] shadow-md shrink-0 rounded" :src="'https://primefaces.org/cdn/primevue/images/product/' + product.image" :alt="product.name" />
                    <div class="flex-1 flex flex-col gap-1">
                        <span class="font-bold">{{ product.name }}</span>
                        <div class="flex items-center gap-2">
                            <i class="pi pi-tag text-sm"></i>
                            <span>{{ product.category }}</span>
                        </div>
                    </div>
                    <span class="font-bold text-surface-900 dark:text-surface-0 ml-5">${{ product.price }}</span>
                </div>
            </li>
        </ul>
        <ContextMenu ref="menu" :model="items" @hide="selectedId = null">
            <template #item="{ item, props }">
                <a v-ripple class="flex items-center" v-bind="props.action">
                    <span :class="item.icon" />
                    <span class="ml-2">{{ item.label }}</span>
                    <Badge v-if="item.badge" class="ml-auto" :value="item.badge" />
                    <span v-if="item.shortcut" class="ml-auto border border-surface-200 dark:border-surface-700 rounded surface-100 text-xs p-1">{{ item.shortcut }}</span>
                    <i v-if="item.items" class="pi pi-angle-right ml-auto text-primary"></i>
                </a>
            </template>
        </ContextMenu>
    </div>
    <DocSectionCode :code="code" :service="['ProductService']" />
</template>

<script>
import { ProductService } from '@/service/ProductService';

export default {
    data() {
        return {
            products: null,
            selectedId: null,
            items: [
                {
                    label: 'Favorite',
                    icon: 'pi pi-star',
                    shortcut: '⌘+D'
                },
                {
                    label: 'Add',
                    icon: 'pi pi-shopping-cart',
                    shortcut: '⌘+A'
                },
                {
                    separator: true
                },
                {
                    label: 'Share',
                    icon: 'pi pi-share-alt',
                    items: [
                        {
                            label: 'Whatsapp',
                            icon: 'pi pi-whatsapp',
                            badge: 2
                        },
                        {
                            label: 'Instagram',
                            icon: 'pi pi-instagram',
                            badge: 3
                        }
                    ]
                }
            ],
            code: {
                basic: `
<ul class="m-0 p-0 list-none border border-surface-200 dark:border-surface-700 rounded p-3 flex flex-col gap-2 w-full md:w-[30rem]">
    <li
        v-for="product in products"
        :key="product.id"
        :class="['p-2 hover:bg-primary-50 dark:hover:bg-primary-400/30 rounded border transition-all duration-200', { 'border-transparent': selectedId !== product.id }, { 'border-primary-500 dark:border-primary-400': selectedId === product.id }]"
        @contextmenu="onRightClick($event, product.id)"
    >
        <div class="flex flex-wrap p-2 items-center gap-3">
            <img class="w-[4rem] shadow-md shrink-0 rounded" :src="'/images/product/' + product.image" :alt="product.name" />
            <div class="flex-1 flex flex-col gap-1">
                <span class="font-bold">{{ product.name }}</span>
                <div class="flex items-center gap-2">
                    <i class="pi pi-tag text-sm"></i>
                    <span>{{ product.category }}</span>
                </div>
            </div>
            <span class="font-bold text-surface-900 dark:text-surface-0 ml-5">\${{ product.price }}</span>
        </div>
    </li>
</ul>
<ContextMenu ref="menu" :model="items" @hide="selectedId = null">
    <template #item="{ item, props }">
        <a v-ripple class="flex items-center" v-bind="props.action">
            <span :class="item.icon" />
            <span class="ml-2">{{ item.label }}</span>
            <Badge v-if="item.badge" class="ml-auto" :value="item.badge" />
            <span v-if="item.shortcut" class="ml-auto border border-surface-200 dark:border-surface-700 rounded surface-100 text-xs p-1">{{ item.shortcut }}</span>
            <i v-if="item.items" class="pi pi-angle-right ml-auto text-primary"></i>
        </a>
    </template>
</ContextMenu>
`,
                options: `
<template>
    <div class="card flex md:justify-center">
        <ul class="m-0 p-0 list-none border border-surface-200 dark:border-surface-700 rounded p-3 flex flex-col gap-2 w-full md:w-[30rem]">
            <li
                v-for="product in products"
                :key="product.id"
                :class="['p-2 hover:bg-primary-50 dark:hover:bg-primary-400/30 rounded border transition-all duration-200', { 'border-transparent': selectedId !== product.id }, { 'border-primary-500 dark:border-primary-400': selectedId === product.id }]"
                @contextmenu="onRightClick($event, product.id)"
            >
                <div class="flex flex-wrap p-2 items-center gap-3">
                    <img class="w-[4rem] shadow-md shrink-0 rounded" :src="'https://primefaces.org/cdn/primevue/images/product/' + product.image" :alt="product.name" />
                    <div class="flex-1 flex flex-col gap-1">
                        <span class="font-bold">{{ product.name }}</span>
                        <div class="flex items-center gap-2">
                            <i class="pi pi-tag text-sm"></i>
                            <span>{{ product.category }}</span>
                        </div>
                    </div>
                    <span class="font-bold text-surface-900 dark:text-surface-0 ml-5">\${{ product.price }}</span>
                </div>
            </li>
        </ul>
        <ContextMenu ref="menu" :model="items" @hide="selectedId = null">
            <template #item="{ item, props }">
                <a v-ripple class="flex items-center" v-bind="props.action">
                    <span :class="item.icon" />
                    <span class="ml-2">{{ item.label }}</span>
                    <Badge v-if="item.badge" class="ml-auto" :value="item.badge" />
                    <span v-if="item.shortcut" class="ml-auto border border-surface-200 dark:border-surface-700 rounded surface-100 text-xs p-1">{{ item.shortcut }}</span>
                    <i v-if="item.items" class="pi pi-angle-right ml-auto text-primary"></i>
                </a>
            </template>
        </ContextMenu>
    </div>
</template>

<script>
import { ProductService } from '@/service/ProductService';

export default {
    data() {
        return {
            products: null,
            selectedId: null,
            items: [
                {
                    label: 'Favorite',
                    icon: 'pi pi-star',
                    shortcut: '⌘+D'
                },
                {
                    label: 'Add',
                    icon: 'pi pi-shopping-cart',
                    shortcut: '⌘+A'
                },
                {
                    separator: true
                },
                {
                    label: 'Share',
                    icon: 'pi pi-share-alt',
                    items: [
                        {
                            label: 'Whatsapp',
                            icon: 'pi pi-whatsapp',
                            badge: 2
                        },
                        {
                            label: 'Instagram',
                            icon: 'pi pi-instagram',
                            badge: 3
                        }
                    ]
                }
            ]
        };
    },
    mounted() {
        ProductService.getProductsSmall().then((data) => (this.products = data));
    },
    methods: {
        onRightClick(event, id) {
            this.selectedId = id;
            this.$refs.menu.show(event);
        }
    }
};
<\/script>
`,
                composition: `
<template>
    <div class="card flex md:justify-center">
        <ul class="m-0 p-0 list-none border border-surface-200 dark:border-surface-700 rounded p-3 flex flex-col gap-2 w-full md:w-[30rem]" @hide="selectedId = null">
            <li
                v-for="product in products"
                :key="product.id"
                :class="['p-2 hover:bg-primary-50 dark:hover:bg-primary-400/30 rounded border transition-all duration-200', { 'border-transparent': selectedId !== product.id }, { 'border-primary-500 dark:border-primary-400': selectedId === product.id }]"
                @contextmenu="onRightClick($event, product.id)"
            >
                <div class="flex flex-wrap p-2 items-center gap-3">
                    <img class="w-[4rem] shadow-md shrink-0 rounded" :src="'https://primefaces.org/cdn/primevue/images/product/' + product.image" :alt="product.name" />
                    <div class="flex-1 flex flex-col gap-1">
                        <span class="font-bold">{{ product.name }}</span>
                        <div class="flex items-center gap-2">
                            <i class="pi pi-tag text-sm"></i>
                            <span>{{ product.category }}</span>
                        </div>
                    </div>
                    <span class="font-bold text-surface-900 dark:text-surface-0 ml-5">\${{ product.price }}</span>
                </div>
            </li>
        </ul>
        <ContextMenu ref="menu" :model="items">
            <template #item="{ item, props }">
                <a v-ripple class="flex items-center" v-bind="props.action">
                    <span :class="item.icon" />
                    <span class="ml-2">{{ item.label }}</span>
                    <Badge v-if="item.badge" class="ml-auto" :value="item.badge" />
                    <span v-if="item.shortcut" class="ml-auto border border-surface-200 dark:border-surface-700 rounded surface-100 text-xs p-1">{{ item.shortcut }}</span>
                    <i v-if="item.items" class="pi pi-angle-right ml-auto text-primary"></i>
                </a>
            </template>
        </ContextMenu>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { ProductService } from '@/service/ProductService'

const menu = ref();
const items = ref([
    {
        label: 'Favorite',
        icon: 'pi pi-star',
        shortcut: '⌘+D'
    },
    {
        label: 'Add',
        icon: 'pi pi-shopping-cart',
        shortcut: '⌘+A'
    },
    {
        separator: true
    },
    {
        label: 'Share',
        icon: 'pi pi-share-alt',
        items: [
            {
                label: 'Whatsapp',
                icon: 'pi pi-whatsapp',
                badge: 2
            },
            {
                label: 'Instagram',
                icon: 'pi pi-instagram',
                badge: 3
            }
        ]
    }
]);

const products = ref(null);
const selectedId = ref(null);

onMounted(() => {
    ProductService.getProductsSmall().then((data) => (products.value = data));
});

const onRightClick = (event, id) => {
    selectedId.value = id;
    menu.value.show(event);
};

<\/script>
`
            },
            data: `
/* ProductService */        
{
    id: '1000',
    code: 'f230fh0g3',
    name: 'Bamboo Watch',
    description: 'Product Description',
    image: 'bamboo-watch.jpg',
    price: 65,
    category: 'Accessories',
    quantity: 24,
    inventoryStatus: 'INSTOCK',
    rating: 5
},
...`
        };
    },
    mounted() {
        ProductService.getProductsMini().then((data) => (this.products = data));
    },
    methods: {
        onRightClick(event, id) {
            this.selectedId = id;
            this.$refs.menu.show(event);
        }
    }
};
</script>
