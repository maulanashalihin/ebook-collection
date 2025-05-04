<script>
  import { onMount } from 'svelte';

  // State for ebooks
  let ebooks = [];
  let loading = true;
  let error = null;
  let searchQuery = '';
  let selectedEbook = null;
  let showModal = false;

  // Animation states
  let hoveredCard = null;

  // Function to fetch ebooks from the public/ebooks directory
  async function fetchEbooks() {
    try {
      // In a real app, you would fetch this from an API
      // For now, we'll hardcode the ebooks we found in the directory
      const ebooksList = [
        {
          id: 1,
          title: 'AI Marketing Secret 2025',
          filename: 'marketing-secret.pdf',
          size: '2.5 MB',
          coverImage: '/covers/marketing-secret.png', // Placeholder image
          description: 'Strategi Praktis Menaklukkan Era Algoritma',
          downloadUrl: '/ebooks/marketing-secret.pdf'
        },
        {
          id: 2,
          title: 'TRIPLE POWER AI AUTOMATION PLAN',
          filename: 'triple-power.pdf',
          size: '2.9 MB',
          coverImage: '/covers/triple-power.png', // Placeholder image
          description: 'Dari Landing Page ke Closing Deal dalam 60 Detik!',
          downloadUrl: '/ebooks/triple-power.pdf'
        },
        {
          id: 3,
          title: 'GREAT LEADERSHIPS',
          filename: 'great-leadership.pdf',
          size: '3.0 MB',
          coverImage: '/covers/great-leadership.png',
          description: 'Membangun Kepemimpinan Tangguh di Era Perubahan',
          downloadUrl: '/ebooks/great-leadership.pdf'
        },
        {
          id: 4,
          title: 'MEMULAI BISNIS',
          filename: 'memulai-bisnis.pdf',
          size: '3.3 MB',
          coverImage: '/covers/memulai-bisnis.png',
          description: 'Panduan Nyata untuk Pemula yang Belum Pernah Berbisnis',
          downloadUrl: '/ebooks/memulai-bisnis.pdf'
        }
      ];
      
      ebooks = ebooksList;
      loading = false;
    } catch (err) {
      console.error('Error fetching ebooks:', err);
      error = 'Failed to load ebooks. Please try again later.';
      loading = false;
    }
  }

  // Function to filter ebooks based on search query
  $: filteredEbooks = searchQuery 
    ? ebooks.filter(ebook => 
        ebook.title.toLowerCase().includes(searchQuery.toLowerCase()) ||
        ebook.description.toLowerCase().includes(searchQuery.toLowerCase())
      )
    : ebooks;

  // Load ebooks on component mount
  onMount(() => {
    fetchEbooks();
  });
</script>

<main class="min-h-screen bg-[#f8f5f0] p-4 md:p-8 bg-[url('https://www.transparenttextures.com/patterns/paper.png')]">
  <div class="max-w-6xl mx-auto">
    <!-- Header with book-like styling -->
    <header class="mb-12 text-center">
      <div class="mb-8 inline-block">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-16 w-16 mx-auto text-[#8B4513]" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" />
        </svg>
      </div>
      <h1 class="text-4xl md:text-6xl font-serif font-bold text-[#5D4037] mb-4">
        book.tapsite.ai
      </h1>
      <p class="text-[#8D6E63] text-xl max-w-2xl mx-auto font-serif">
        Your personal library of premium ebooks
      </p>
      
      <!-- Search Bar with bookish styling -->
      <div class="mt-8 max-w-md mx-auto">
        <div class="relative">
          <input
            type="text"
            bind:value={searchQuery}
            placeholder="Search your library..."
            class="w-full px-4 py-3 rounded-lg border-2 border-[#D7CCC8] focus:border-[#8D6E63] focus:outline-none bg-[#FFF8E1] text-[#5D4037] shadow-md transition-all duration-300 placeholder-[#BCAAA4] font-serif"
          />
          <div class="absolute right-3 top-3 text-[#8D6E63]">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
            </svg>
          </div>
        </div>
      </div>
    </header>

    <!-- Loading State -->
    {#if loading}
      <div class="flex justify-center items-center h-64">
        <div class="animate-spin rounded-full h-16 w-16 border-t-4 border-b-4 border-[#8D6E63]"></div>
      </div>
    {:else if error}
      <div class="bg-red-100 text-red-700 p-4 rounded-lg text-center">
        {error}
      </div>
    <!-- Ebooks Grid with book-like styling -->
    {:else if filteredEbooks.length > 0}
      <div class="grid grid-cols-2 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4 md:gap-8">
        {#each filteredEbooks as ebook (ebook.id)}
          <div 
            class="flex flex-col bg-[#FFF8E1] rounded-lg overflow-hidden shadow-lg border border-[#D7CCC8] transition-all duration-300 hover:shadow-xl"
            on:mouseenter={() => hoveredCard = ebook.id}
            on:mouseleave={() => hoveredCard = null}
          >
            <!-- Book Cover with 3D effect -->
            <div class="relative mx-auto pt-4 pb-2 px-2 md:pt-6 md:pb-2 md:px-4">
              <div class="book-cover-container">
                <img 
                  src={ebook.coverImage} 
                  alt={ebook.title} 
                  class="book-cover shadow-lg"
                />
                <div class="book-spine"></div>
                <div class="book-side"></div>
              </div>
            </div>
            
            <!-- Book Info -->
            <div class="p-2 md:p-4 flex-grow flex flex-col">
              <h3 class="font-serif font-medium text-[#5D4037] text-sm md:text-lg text-center mb-1 md:mb-2">{ebook.title}</h3>
              <p class="text-xs md:text-sm text-[#8D6E63] line-clamp-2 font-serif text-center mb-2 md:mb-4">{ebook.description}</p>
              
              <div class="mt-auto">
                <div class="text-xs text-[#8D6E63] font-serif text-center mb-2 md:mb-3">
                  {ebook.size}
                </div>
                
                <a 
                  href={ebook.downloadUrl} 
                  download
                  class="block w-full bg-[#8D6E63] hover:bg-[#5D4037] text-[#FFF8E1] py-1.5 md:py-2 rounded-lg transition-colors flex items-center justify-center gap-1 shadow-md font-serif text-xs md:text-sm"
                >
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3 md:h-4 md:w-4" viewBox="0 0 20 20" fill="currentColor">
                    <path fill-rule="evenodd" d="M3 17a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm3.293-7.707a1 1 0 011.414 0L9 10.586V3a1 1 0 112 0v7.586l1.293-1.293a1 1 0 111.414 1.414l-3 3a1 1 0 01-1.414 0l-3-3a1 1 0 010-1.414z" clip-rule="evenodd" />
                  </svg>
                  Download
                </a>
              </div>
            </div>
          </div>
        {/each}
      </div>
      
      <!-- Quote for book lovers -->
      <div class="mt-16 text-center italic text-[#8D6E63] font-serif">
        <p class="text-lg">"A reader lives a thousand lives before he dies. The man who never reads lives only one."</p>
        <p class="text-sm mt-2">— George R.R. Martin</p>
      </div>
    {:else}
      <div class="text-center py-12">
        <p class="text-[#8D6E63] text-xl font-serif">No ebooks found matching your search.</p>
      </div>
    {/if}
  </div>
</main>

<style>
  /* Additional custom styles */
  .line-clamp-2 {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
  
  /* Book cover 3D effect */
  .book-cover-container {
    position: relative;
    perspective: 1000px;
    transform-style: preserve-3d;
    transition: transform 0.5s ease;
  }
  
  .book-cover-container:hover {
    transform: rotateY(-25deg);
  }
  
  .book-cover {
    width: 140px;
    height: 200px;
    object-fit: cover;
    border-radius: 2px;
    position: relative;
    transform-origin: 0% 50%;
    transition: transform 0.5s ease;
  }
  
  @media (min-width: 768px) {
    .book-cover {
      width: 180px;
      height: 260px;
    }
  }
  
  .book-spine {
    position: absolute;
    top: 0;
    left: 0;
    width: 10px;
    height: 100%;
    background: linear-gradient(to right, rgba(0,0,0,0.2), rgba(0,0,0,0.05));
    transform: translateX(-10px) rotateY(-90deg);
    transform-origin: 100% 50%;
  }
  
  .book-side {
    position: absolute;
    top: 0;
    right: 0;
    width: 3px;
    height: 100%;
    background: linear-gradient(to left, rgba(0,0,0,0.1), rgba(0,0,0,0.05));
    transform: translateX(3px) rotateY(90deg);
    transform-origin: 0% 50%;
  }
  
  /* Add some subtle texture and warmth to create a cozy reading environment */
  :global(body) {
    font-family: 'Georgia', serif;
  }
</style>