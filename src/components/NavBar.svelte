<script lang="ts">
	import {
		Navbar,
		NavBrand,
		NavUl,
		NavLi,
		NavHamburger,
		Dropdown,
		DropdownItem,
		Chevron,
		DropdownDivider
	} from 'flowbite-svelte';
	import { category_slug, loggedAs, rights } from '../store';
	import { goto } from '$app/navigation';

	let handleCategoryChange = (category: string) => {
		category_slug.set(category);
	};

	const handleLogout = () => {
		loggedAs.set('Anonymous');
		rights.set('USER');
		goto('/');
	};
</script>

<Navbar let:hidden let:toggle color="blue">
	<NavBrand href="/">
		<img src="https://jxdsmvzqgqofgdetgfha.supabase.co/storage/v1/object/public/default/static/AppLogo.png?t=2023-08-09T12%3A48%3A06.031Z" class="mr-3 h-6 sm:h-9" alt="ShareIt logo" />
		<span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white">
			ShareIt
		</span>
	</NavBrand>
	<NavHamburger on:click={toggle} />
	<NavUl {hidden}>
		<NavLi href="/">Home</NavLi>
		<NavLi href="/stories">Stories</NavLi>
		<NavLi href="/about">About Us</NavLi>
		<NavLi href="/contact">Contact Us</NavLi>
		<NavLi id="nav-menu1" class="cursor-pointer"><Chevron aligned>Categories</Chevron></NavLi>
		<Dropdown triggeredBy="#nav-menu1" class="w-44 z-20">
			<DropdownItem href="/categories/family" on:click={() => handleCategoryChange('family')}
				>Family</DropdownItem
			>
			<DropdownItem href="/categories/friends" on:click={() => handleCategoryChange('friends')}
				>Friends</DropdownItem
			>
			<DropdownItem href="/categories/pets" on:click={() => handleCategoryChange('pets')}
				>Pets</DropdownItem
			>
			<DropdownItem href="/categories/romance" on:click={() => handleCategoryChange('romance')}
				>Romance</DropdownItem
			>
			<DropdownItem href="/categories/workplace" on:click={() => handleCategoryChange('workplace')}
				>Work Place</DropdownItem
			>
			<DropdownDivider />
			<DropdownItem href="/categories/general" on:click={() => handleCategoryChange('general')}
				>General</DropdownItem
			>
		</Dropdown>
		{#if $loggedAs === 'Anonymous'}
			<NavLi href="/login">Login</NavLi>
		{:else}
			<NavLi id="nav-menu2" class="cursor-pointer"
				><Chevron aligned>Logged as {$loggedAs}</Chevron></NavLi
			>
			<Dropdown triggeredBy="#nav-menu2" class="w-44 z-20">
				<DropdownItem href="/account">Account</DropdownItem>
				<DropdownItem on:click={handleLogout}>Log Out</DropdownItem>
			</Dropdown>
		{/if}
	</NavUl>
</Navbar>
