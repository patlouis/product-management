<script setup lang="ts">
import { Head, Link } from '@inertiajs/vue3'; 
import AppLayout from '@/layouts/AppLayout.vue';
import Button from '@/components/ui/button/Button.vue';
import { type BreadcrumbItem, type Category } from '@/types';
import { route } from 'ziggy-js';

interface Props {
    categories: {
        data: Category[];
        links: any[];
    };
    breadcrumbs?: BreadcrumbItem[];
}

const props = defineProps<Props>();

const breadcrumbs: BreadcrumbItem[] = [
    { title: 'Categories', href: route('categories.index') },
];

const confirmDelete = () => {
    return window.confirm('Are you sure you want to delete this category?');
};
</script>

<template>
    <Head title="Categories" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div class="space-y-2 p-6">
            <div class="flex items-center justify-between py-4">
                <div>
                    <h1 class="text-2xl font-bold tracking-tight text-zinc-900 dark:text-zinc-100">Category List</h1>
                    <p class="text-sm text-zinc-500">Manage your product categories.</p>
                </div>
                
                <Link :href="route('categories.create')">
                    <Button class="gap-2 cursor-pointer">
                        <span class="text-lg">+</span> 
                        Create Category
                    </Button>
                </Link>
            </div>

            <div class="border rounded-lg border-zinc-200 dark:border-zinc-800 overflow-hidden">
                <table class="w-full text-sm text-left">
                    <thead class="bg-zinc-50 dark:bg-zinc-900 text-zinc-500 border-b border-zinc-200 dark:border-zinc-800">
                        <tr>
                            <th class="px-4 py-3 font-medium">Name</th>
                            <th class="px-4 py-3 font-medium">Slug</th>
                            <th class="px-4 py-3 font-medium">Description</th>
                            <th class="px-4 py-3 font-medium text-right">Actions</th>
                        </tr>
                    </thead>
                    <tbody class="divide-y divide-zinc-200 dark:divide-zinc-800 bg-white dark:bg-zinc-950">
                        <tr v-for="category in props.categories.data" :key="category.id" class="hover:bg-zinc-50 dark:hover:bg-zinc-900/50 transition-colors">
                            <td class="px-4 py-3 font-medium text-zinc-900 dark:text-zinc-100">
                                {{ category.name }}
                            </td>
                            <td class="px-4 py-3 text-zinc-600 dark:text-zinc-400 font-mono text-xs">
                                {{ category.slug }}
                            </td>
                            <td class="px-4 py-3 text-zinc-500 dark:text-zinc-500 truncate max-w-xs">
                                {{ category.description || '-' }}
                            </td>
                            <td class="px-4 py-3 text-right">
                                <div class="flex justify-end items-center gap-4">
                                    <Link :href="route('categories.edit', category.id)" class="text-zinc-500 hover:text-indigo-600 text-xs font-medium underline">
                                        Edit
                                    </Link>
                                    <Link 
                                        :href="route('categories.destroy', category.id)" 
                                        method="delete" 
                                        as="button"
                                        type="button"
                                        :onBefore="confirmDelete"                                     
                                        class="text-red-600 hover:text-red-700 text-xs font-medium underline cursor-pointer"
                                    >
                                        Delete
                                    </Link>
                                </div>
                            </td>
                        </tr>

                        <tr v-if="props.categories.data.length === 0">
                            <td colspan="4" class="px-4 py-8 text-center text-zinc-500">
                                No categories found. Click "Create Category" to add one.
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>

            <div v-if="props.categories.data.length > 0" class="flex justify-end gap-2">
                <Link 
                    v-for="link in props.categories.links" 
                    :key="link.label"
                    :href="link.url ?? '#'"
                    :class="[
                        'px-3 py-1 text-xs rounded border',
                        link.active 
                            ? 'bg-zinc-900 text-white border-zinc-900 dark:bg-zinc-100 dark:text-zinc-900' 
                            : 'bg-white text-zinc-600 border-zinc-200 hover:bg-zinc-50 dark:bg-zinc-950 dark:border-zinc-800 dark:text-zinc-400',
                        !link.url && 'opacity-50 cursor-not-allowed'
                    ]"
                    v-html="link.label" 
                />
            </div>
        </div>
    </AppLayout>
</template>
