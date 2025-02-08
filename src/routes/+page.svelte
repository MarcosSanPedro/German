<script lang="ts">
	import { onMount } from 'svelte';
	import { fade, fly } from 'svelte/transition';
	import { writable } from 'svelte/store';
	
	let showContent = false;
	let showServices = Array(7).fill(false);
	let showTestimonials = Array(3).fill(false);
	
	// Create a store for the theme
	const theme = writable('light');
	
	// Function to toggle the theme
	function toggleTheme() {
	  theme.update(currentTheme => currentTheme === 'light' ? 'dark' : 'light');
	}
	
	// Update the document class when the theme changes
	$: if (typeof document !== 'undefined') {
	  document.documentElement.classList.toggle('dark', $theme === 'dark');
	}
	
	onMount(() => {
	  showContent = true;
	  
	  const observer = new IntersectionObserver((entries) => {
		entries.forEach(entry => {
		  if (entry.isIntersecting) {
			const index = parseInt(entry.target.dataset.index);
			if (entry.target.classList.contains('service')) {
			  showServices[index] = true;
			} else if (entry.target.classList.contains('testimonial')) {
			  showTestimonials[index] = true;
			}
		  }
		});
	  }, { threshold: 0.1 });
	
	  document.querySelectorAll('.animate-on-scroll').forEach(el => {
		observer.observe(el);
	  });
	});
	
	const services = [
	  { 
		name: 'Asilos', 
		description: 'Asistencia integral en procesos de asilo político, guiándote desde la preparación del caso hasta la representación ante autoridades migratorias.', 
		whatsapp: 'Me interesa información sobre servicios de asilo.' 
	  },
	  { 
		name: 'Permisos de trabajo', 
		description: 'Facilitamos la obtención de autorizaciones laborales, gestionando toda la documentación necesaria para un proceso eficiente.', 
		whatsapp: 'Quisiera saber más sobre los permisos de trabajo.' 
	  },
	  { 
		name: 'Peticiones familiares', 
		description: 'Te ayudamos a reunir a tu familia en los Estados Unidos, manejando todos los aspectos legales para la reunificación familiar.', 
		whatsapp: 'Necesito ayuda con una petición familiar.' 
	  },
	  { 
		name: 'Residencias', 
		description: 'Te acompañamos en cada etapa para obtener tu Green Card, desde la evaluación inicial hasta la preparación para la entrevista.', 
		whatsapp: 'Me gustaría información sobre cómo obtener la residencia.' 
	  },
	  { 
		name: 'Ciudadanías', 
		description: 'Ofrecemos asesoría completa en el proceso de naturalización, incluyendo preparación para el examen y asistencia en la solicitud.', 
		whatsapp: 'Estoy interesado en el proceso de ciudadanía.' 
	  },
	  { 
		name: 'Notario Público', 
		description: 'Servicios de notario público para certificaciones, autenticaciones y otros servicios notariales esenciales en trámites migratorios.', 
		whatsapp: 'Requiero servicios de notario público.' 
	  },
	  { 
		name: 'Consultoría migratoria', 
		description: 'Asesoramiento personalizado en todos los aspectos de la ley de inmigración, ofreciendo soluciones adaptadas a tu situación.', 
		whatsapp: 'Necesito una consultoría migratoria general.' 
	  }
	];
	
	const testimonials = [
	  { name: 'María González', text: 'PDM Immigration Services hizo posible mi sueño de obtener la Green Card. Su equipo es excepcional.' },
	  { name: 'Juan Pérez', text: 'Gracias a PDM, pude reunir a mi familia en los Estados Unidos. Su dedicación es incomparable.' },
	  { name: 'Ana Rodríguez', text: 'El proceso de ciudadanía fue mucho más sencillo con la guía experta de PDM. Los recomiendo totalmente.' }
	];
	
	function getWhatsAppLink(message: string) {
	  return `https://wa.me/13059264581?text=${encodeURIComponent(message)}`;
	}
	
	function StarRating() {
	  return `
		<div class="flex justify-center mb-4">
		  ${Array(5).fill().map(() => `
			<svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gold" viewBox="0 0 20 20" fill="#FFD700">
			  <path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z" />
			</svg>
		  `).join('')}
		</div>
	  `;
	}
	</script>
	
	<main class="font-sans text-gray-900 dark:text-white bg-gray-100 dark:bg-gradient-to-b dark:from-black dark:to-gray-900 transition-colors duration-300">
	<!-- Navigation -->
	<nav class="bg-white dark:bg-black bg-opacity-80 dark:bg-opacity-80 backdrop-blur-md shadow-md fixed w-full z-50 transition-colors duration-300">
	  <div class="container mx-auto px-6 py-3 flex justify-between items-center">
		<div class="text-xl font-bold text-gray-900 dark:text-gold whitespace-nowrap">PDM IMMIGRATION SERVICES</div>
		<div class="flex items-center space-x-6">
		  <div class="hidden md:flex space-x-6">
			<a href="#servicios" class="text-gray-900 dark:text-white hover:text-gold dark:hover:text-gold transition duration-300">Servicios</a>
			<a href="#testimonios" class="text-gray-900 dark:text-white hover:text-gold dark:hover:text-gold transition duration-300">Testimonios</a>
			<a href="#contacto" class="text-gray-900 dark:text-white hover:text-gold dark:hover:text-gold transition duration-300">Contacto</a>
		  </div>
		  <button on:click={toggleTheme} class="p-2 rounded-full bg-gray-200 dark:bg-gray-700 text-gray-900 dark:text-white transition-colors duration-300">
			{#if $theme === 'light'}
			  🌙
			{:else}
			  ☀️
			{/if}
		  </button>
		</div>
	  </div>
	</nav>
	
	<!-- Hero Section -->
	<section class="relative min-h-screen flex items-center justify-center overflow-hidden">
	  <div class="absolute inset-0 bg-black dark:bg-gradient-to-b dark:from-black dark:to-gray-900 opacity-80 transition-colors duration-300"></div>
	  <div class="relative z-10 text-center px-6 max-w-4xl mx-auto">
		{#if showContent}
		  <h1 in:fly="{{ y: 50, duration: 1000 }}" class="text-5xl md:text-6xl font-bold mb-6  text-oro dark:text-gold">Apoyándote en cada paso de tu camino</h1>
		  <p in:fly="{{ y: 50, duration: 1000, delay: 300 }}" class="text-xl md:text-4xl mb-8 text-white dark:text-white">Te ayudamos a gestionar tus trámites de inmigración, servicios notariales y preparación de impuestos con profesionalismo y confianza.</p>
		  <a in:fly="{{ y: 50, duration: 1000, delay: 600 }}" href="tel:+13059264581" class="bg-gold text-black font-bold py-3 px-8 rounded-full hover:bg-opacity-80 transition duration-300 inline-block text-lg shadow-lg hover:shadow-xl transform hover:-translate-y-1">Consulta Gratis</a>
		{/if}
	  </div>
	  <div class="absolute inset-0 bg-[url(/3.jpg)] bg-cover bg-center  opacity-30 dark:opacity-20"></div>
	</section>
	
	<!-- Services Section -->
	<section id="servicios" class="py-20 bg-white dark:bg-gray-900 transition-colors duration-300">
	  <div class="container mx-auto px-6">
		<h2 class="text-4xl font-bold text-center mb-16 text-gray-900 dark:text-gold">Nuestros Servicios</h2>
		<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-12">
		  {#each services as service, i}
			<div class="animate-on-scroll service" data-index={i}>
			  {#if showServices[i]}
				<div in:fly="{{ y: 50, duration: 1000 }}" class="bg-gray-100 dark:bg-black bg-opacity-60 dark:bg-opacity-60 backdrop-blur-md rounded-lg shadow-xl overflow-hidden transition duration-300 transform hover:-translate-y-2 hover:shadow-2xl h-full flex flex-col border border-gray-300 dark:border-gold">
				  <div class="p-8 flex-grow">
					<h3 class="text-2xl font-semibold mb-4 text-gray-900 dark:text-gold">{service.name}</h3>
					<p class="text-gray-700 dark:text-gray-300 mb-6">{service.description}</p>
				  </div>
				  <div class="p-4 bg-opacity-10">
					<a href={getWhatsAppLink(service.whatsapp)} target="_blank" rel="noopener noreferrer" class="inline-block w-full bg-gold text-black font-bold py-3 px-6 rounded-full hover:bg-opacity-80 transition duration-300 text-center shadow-md hover:shadow-lg transform hover:-translate-y-1">
					  Consultar por WhatsApp
					</a>
				  </div>
				</div>
			  {/if}
			</div>
		  {/each}
		</div>
	  </div>
	</section>
	
	<!-- Special Tax Services Section -->
	<section class="py-20 bg-gold dark:bg-gray-800 text-black dark:text-white transition-colors duration-300">
	  <div class="container mx-auto px-6">
		<div class="flex flex-col md:flex-row items-center justify-between">
		  <div class="md:w-1/2 mb-8 md:mb-0">
			<h2 class="text-4xl font-bold mb-6">Preparación de Impuestos</h2>
			<p class="text-xl mb-8">Ofrecemos servicios profesionales de preparación de impuestos tanto para individuos como para corporaciones. Maximiza tus devoluciones y asegura el cumplimiento fiscal con nuestro equipo experto.</p>
			<a href="tel:+13059264581" class="bg-black dark:bg-gold text-gold dark:text-black font-bold py-3 px-8 rounded-full hover:bg-opacity-80 transition duration-300 inline-block text-lg shadow-lg hover:shadow-xl transform hover:-translate-y-1">
			  Consulta  Gratuita
			</a>
		  </div>
		  <div class="md:w-1/2 flex justify-center">
			<div class="bg-white dark:bg-gray-900 p-8 rounded-full shadow-2xl transform hover:rotate-3 transition duration-300">
			  <svg xmlns="http://www.w3.org/2000/svg" class="h-32 w-32 text-gold dark:text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 7h6m0 10v-3m-3 3h.01M9 17h.01M9 14h.01M12 14h.01M15 11h.01M12 11h.01M9 11h.01M7 21h10a2 2 0 002-2V5a2 2 0 00-2-2H7a2 2 0 00-2 2v14a2 2 0 002 2z" />
			  </svg>
			</div>
		  </div>
		</div>
	  </div>
	</section>
	
	<!-- Testimonials Section -->
	<section id="testimonios" class="py-20 bg-gray-100 dark:bg-black bg-opacity-60 dark:bg-opacity-60 transition-colors duration-300">
	  <div class="container mx-auto px-6">
		<h2 class="text-4xl font-bold text-center mb-16 text-gray-900 dark:text-gold">Testimonios</h2>
		<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-12">
		  {#each testimonials as testimonial, i}
			<div class="animate-on-scroll testimonial" data-index={i}>
			  {#if showTestimonials[i]}
				<div in:fly="{{ y: 50, duration: 1000 }}" class="bg-white dark:bg-gray-900 bg-opacity-60 dark:bg-opacity-60 backdrop-blur-md text-gray-900 dark:text-white rounded-lg shadow-xl p-6 h-full border border-gray-300 dark:border-gold flex flex-col">
				  {@html StarRating()}
				  <p class="italic mb-4 text-lg flex-grow">"{testimonial.text}"</p>
				  <p class="font-semibold text-right text-gray-900 dark:text-gold">- {testimonial.name}</p>
				</div>
			  {/if}
			</div>
		  {/each}
		</div>
	  </div>
	</section>
	
	<!-- Contact Section -->
	<section id="contacto" class="py-20 bg-white dark:bg-gray-900 bg-opacity-60 dark:bg-opacity-60 transition-colors duration-300">
	  <div class="container mx-auto px-6 text-center">
		<h2 class="text-4xl font-bold mb-12 text-gray-900 dark:text-gold">Contáctanos</h2>
		<p class="text-xl mb-8 text-gray-800 dark:text-white">Estamos aquí para ayudarte con tus necesidades migratorias.</p>
		<p class="text-3xl font-bold mb-12 dark:text-gold">+1 (305) 926-4581</p>
		<div class="flex justify-center space-x-8">
		  <a href="https://www.tiktok.com/@pdm_immigrationservices" target="_blank" rel="noopener noreferrer" class="bg-gold text-black p-4 rounded-full hover:bg-opacity-80 transition duration-300 transform hover:-translate-y-1 shadow-md hover:shadow-lg">
			<svg class="w-8 h-8" fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M19.59 6.69a4.83 4.83 0 0 1-3.77-4.25V2h-3.45v13.67a2.89 2.89 0 0 1-5.2 1.74 2.89 2.89 0 0 1 2.31-4.64 2.93 2.93 0 0 1 .88.13V9.4a6.84 6.84 0 0 0-1-.05A6.33 6.33 0 0 0 5 20.1a6.34 6.34 0 0 0 10.86-4.43v-7a8.16 8.16 0 0 0 4.77 1.52v-3.4a4.85 4.85 0 0 1-1-.1z"/></svg>
		  </a>
		  <a href="https://www.facebook.com/profile.php?id=100088402383049" target="_blank" rel="noopener noreferrer" class="bg-gold text-black p-4 rounded-full hover:bg-opacity-80 transition duration-300 transform hover:-translate-y-1 shadow-md hover:shadow-lg">
			<svg class="w-8 h-8" fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M24 12.07C24 5.41 18.63 0 12 0S0 5.4 0 12.07C0 18.1 4.39 23.1 10.13 24v-8.44H7.08v-3.49h3.04V9.41c0-3.02 1.8-4.7 4.54-4.7 1.31 0 2.68.24 2.68.24v2.97h-1.5c-1.5 0-1.96.93-1.96 1.89v2.26h3.32l-.53 3.5h-2.8V24C19.62 23.1 24 18.1 24 12.07"/></svg>
		  </a>
		</div>
	  </div>
	</section>
	
	<!-- Footer -->
	<footer class="bg-gray-200 dark:bg-black bg-opacity-80 dark:bg-opacity-80 dark:text-white py-8 transition-colors duration-300">
	  <div class="container mx-auto px-6 text-center">
		<p>&copy; {new Date().getFullYear()} PDM MULTISERVICES LLC. Todos los derechos reservados.</p>
	  </div>
	</footer>
	</main>
	
	<style lang="postcss">
	:global(html) {
	  scroll-behavior: smooth;
	}
	
	:global(:root) {
	  --color-gold: #FFD700;
	}
	
	:global(body) {
	  @apply bg-gray-100 dark:bg-black transition-colors duration-300;
	}
	
	:global(.text-gold) {
	  color: var(--color-gold);
	}
	</style>
	
	