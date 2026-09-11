<script setup>
import { Head, Link } from '@inertiajs/vue3';
import { ref, computed } from 'vue';

defineProps({
    canLogin: {
        type: Boolean,
        default: true,
    },
    canRegister: {
        type: Boolean,
        default: true,
    },
    laravelVersion: {
        type: String,
        default: '13.x',
    },
    phpVersion: {
        type: String,
        default: '8.5',
    },
});

// Mobile menu state
const mobileMenuOpen = ref(false);

// 20+ Household Waste Database (PRD Section 7.1)
const wasteList = [
    // ORGANIK
    {
        id: 1,
        name: 'Kulit Buah & Pisang',
        category: 'Organik',
        badgeColor: 'bg-emerald-100 text-emerald-800 border-emerald-200',
        handling: 'Olah menjadi kompos atau eco-enzyme. Masukkan ke komposter atau tong sampah hijau.',
    },
    {
        id: 2,
        name: 'Sisa Sayuran Dapur',
        category: 'Organik',
        badgeColor: 'bg-emerald-100 text-emerald-800 border-emerald-200',
        handling: 'Tiriskan kuah terlebih dahulu, jadikan pupuk kompos organik atau pakan budidaya maggot BSF.',
    },
    {
        id: 3,
        name: 'Daun Kering & Ranting',
        category: 'Organik',
        badgeColor: 'bg-emerald-100 text-emerald-800 border-emerald-200',
        handling: 'Unsur karbon ideal untuk campuran kompos. Hindari membakar karena menimbulkan polusi udara.',
    },
    {
        id: 4,
        name: 'Ampas Kopi & Teh',
        category: 'Organik',
        badgeColor: 'bg-emerald-100 text-emerald-800 border-emerald-200',
        handling: 'Dapat langsung ditabur tipis ke media tanah tanaman sebagai pupuk penyubur alami.',
    },
    {
        id: 5,
        name: 'Cangkang Telur',
        category: 'Organik',
        badgeColor: 'bg-emerald-100 text-emerald-800 border-emerald-200',
        handling: 'Remas hingga menjadi serpihan halus, tabur di pot bunga atau tanah sebagai sumber kalsium alami.',
    },
    {
        id: 6,
        name: 'Nasi Sisa / Basi',
        category: 'Organik',
        badgeColor: 'bg-emerald-100 text-emerald-800 border-emerald-200',
        handling: 'Bisa dibuat Mikroorganisme Lokal (MOL) untuk bioaktivator kompos atau dijemur untuk pakan unggas.',
    },
    {
        id: 7,
        name: 'Tulang Ayam & Duri Ikan',
        category: 'Organik',
        badgeColor: 'bg-emerald-100 text-emerald-800 border-emerald-200',
        handling: 'Masukkan ke lubang biopori atau haluskan untuk suplemen fosfor tanaman.',
    },

    // ANORGANIK
    {
        id: 8,
        name: 'Botol Plastik PET (Air Mineral)',
        category: 'Anorganik',
        badgeColor: 'bg-blue-100 text-blue-800 border-blue-200',
        handling: 'Kosongkan sisa air, lepas label, remas pipih, kumpulkan dan setor ke Bank Sampah terdekat.',
    },
    {
        id: 9,
        name: 'Kardus Box Bekas Paket',
        category: 'Anorganik',
        badgeColor: 'bg-blue-100 text-blue-800 border-blue-200',
        handling: 'Lepaskan selotip/lakban yang menempel, lipat rata dan simpan di area kering sebelum disetor.',
    },
    {
        id: 10,
        name: 'Kaleng Minuman Aluminium',
        category: 'Anorganik',
        badgeColor: 'bg-blue-100 text-blue-800 border-blue-200',
        handling: 'Bilas bersih dari sisa sirup/soda, pipihkan untuk hemat tempat, pilah untuk didaur ulang.',
    },
    {
        id: 11,
        name: 'Kertas HVS & Majalah Bekas',
        category: 'Anorganik',
        badgeColor: 'bg-blue-100 text-blue-800 border-blue-200',
        handling: 'Jaga tetap kering dan tidak terkena minyak. Ikat rapi untuk ditimbang di Bank Sampah.',
    },
    {
        id: 12,
        name: 'Botol & Stoples Kaca',
        category: 'Anorganik',
        badgeColor: 'bg-blue-100 text-blue-800 border-blue-200',
        handling: 'Cuci bersih dan simpan tanpa dipecahkan agar aman disetor kembali ke bank daur ulang.',
    },
    {
        id: 13,
        name: 'Kantong Kresek Bersih',
        category: 'Anorganik',
        badgeColor: 'bg-blue-100 text-blue-800 border-blue-200',
        handling: 'Gunakan kembali untuk belanja (reuse). Jika sobek, kumpulkan dalam bundel plastik khusus.',
    },

    // B3 (Bahan Berbahaya dan Beracun)
    {
        id: 14,
        name: 'Baterai Bekas (AA/AAA/HP)',
        category: 'B3',
        badgeColor: 'bg-rose-100 text-rose-800 border-rose-200',
        handling: 'BAHAYA! Jangan dibakar atau dibuang ke tempat sampah umum. Bawa ke Drop Point Limbah B3/TPS3R.',
    },
    {
        id: 15,
        name: 'Lampu Neon / Bohlam Rusak',
        category: 'B3',
        badgeColor: 'bg-rose-100 text-rose-800 border-rose-200',
        handling: 'Mengandung uap merkuri. Bungkus kardus/koran agar tidak pecah dan salurkan ke titik kumpul B3.',
    },
    {
        id: 16,
        name: 'Minyak Jelantah Sisa Dapur',
        category: 'B3',
        badgeColor: 'bg-rose-100 text-rose-800 border-rose-200',
        handling: 'Jangan dibuang ke wastafel! Saring, simpan dalam jeriken/botol, setor ke mitra pengolah biodiesel.',
    },
    {
        id: 17,
        name: 'Obat Kedaluwarsa & Strip Pil',
        category: 'B3',
        badgeColor: 'bg-rose-100 text-rose-800 border-rose-200',
        handling: 'Hancurkan isi obat, pisahkan kemasannya, dan salurkan lewat program drop-box obat terdekat.',
    },
    {
        id: 18,
        name: 'Kaleng Semprot Obat Nyamuk / Aerosol',
        category: 'B3',
        badgeColor: 'bg-rose-100 text-rose-800 border-rose-200',
        handling: 'Mudah meledak bila terkena panas. Pastikan gas habis sebelum diserahkan ke pengelola B3.',
    },

    // RESIDU
    {
        id: 19,
        name: 'Popok Bayi & Pembalut Sekali Pakai',
        category: 'Residu',
        badgeColor: 'bg-slate-200 text-slate-800 border-slate-300',
        handling: 'Bersihkan kotoran padat ke kloset, gulung dan bungkus plastik rapat sebelum masuk tempat sampah residu ke TPA.',
    },
    {
        id: 20,
        name: 'Masker Medis Sekali Pakai',
        category: 'Residu',
        badgeColor: 'bg-slate-200 text-slate-800 border-slate-300',
        handling: 'Gunting tali masker untuk lindungi satwa liar, masukkan kantong terikat, buang ke tong residu.',
    },
    {
        id: 21,
        name: 'Tisu Kotor & Tisu Basah',
        category: 'Residu',
        badgeColor: 'bg-slate-200 text-slate-800 border-slate-300',
        handling: 'Serat kertas sudah hancur dan tercemar, tidak dapat didaur ulang. Masukkan ke tong residu.',
    },
    {
        id: 22,
        name: 'Styrofoam Makanan Berminyak',
        category: 'Residu',
        badgeColor: 'bg-slate-200 text-slate-800 border-slate-300',
        handling: 'Styrofoam yang kotor minyak sulit didaur ulang secara ekonomis. Masukkan ke kategori residu.',
    },
    {
        id: 23,
        name: 'Puntung Rokok',
        category: 'Residu',
        badgeColor: 'bg-slate-200 text-slate-800 border-slate-300',
        handling: 'Pastikan bara sudah padam total. Jangan dibuang sembarangan karena mengandung racun residu.',
    },
];

// Interactive Filter & Search
const searchQuery = ref('');
const selectedCategory = ref('all');

const filteredWaste = computed(() => {
    return wasteList.filter((item) => {
        const matchesCategory =
            selectedCategory.value === 'all' ||
            item.category.toLowerCase() === selectedCategory.value.toLowerCase();
        const matchesQuery =
            item.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
            item.category.toLowerCase().includes(searchQuery.value.toLowerCase());
        return matchesCategory && matchesQuery;
    });
});

function setQuery(text) {
    searchQuery.value = text;
}

// Interactive PilahAI Simulator (Gemini API Function Calling Showcase)
const chatMessages = ref([
    {
        sender: 'bot',
        text: 'Halo! Saya <strong>PilahAI</strong> 🌿.<br>Saya siap membantu Anda memilah sampah, mencari lokasi Bank Sampah atau TPS terdekat, melihat jadwal angkut wilayah, hingga merekomendasikan penyaluran sampah. Apa yang ingin Anda tanyakan?',
        functionCall: null,
    },
]);

const userInput = ref('');
const isTyping = ref(false);

const presetScenarios = {
    baterai: {
        userText: 'Baterai bekas masuk kategori apa & bagaimana buangnya?',
        functionCall: "cekKategoriSampah('baterai') -> rekomendasiPenyaluran('baterai')",
        reply: 'Baterai bekas termasuk kategori <strong>Limbah B3</strong> (Bahan Berbahaya dan Beracun) ⚠️.<br><br><strong>Cara Penanganan:</strong><br>1. Jangan dibakar atau dibuang ke tempat sampah biasa.<br>2. Simpan di wadah kering tertutup khusus.<br>3. Salurkan ke <strong>Drop Point E-Waste / TPS3R</strong> terdekat agar logam berat tidak meracuni air tanah.',
    },
    lokasi: {
        userText: 'Di mana Bank Sampah terdekat dari sini?',
        functionCall: "cariFasilitasTerdekat(gps: true, radius: '2km')",
        reply: 'Berdasarkan koordinat lokasi Anda, ditemukan:<br><br>📍 <strong>Bank Sampah Berkah Sejahtera</strong> (Jarak: ± 650m)<br>• <strong>Alamat:</strong> Jl. Mawar No. 14, RT 02/RW 04<br>• <strong>Jadwal Buka:</strong> Sabtu & Minggu, 08.00 - 12.00 WIB<br>• <strong>Sampah Diterima:</strong> Botol Plastik, Kardus, Kaleng, dan Minyak Jelantah.',
    },
    jadwal: {
        userText: 'Kapan jadwal angkut sampah organik di wilayah Tembalang?',
        functionCall: "cekJadwalAngkut(wilayah: 'Tembalang', jenis: 'Organik')",
        reply: 'Jadwal pengangkutan sampah <strong>Organik</strong> untuk wilayah <strong>Tembalang</strong>:<br><br>🗓️ <strong>Setiap Selasa & Jumat</strong><br>⏰ <strong>Pukul 07.00 - 09.30 WIB</strong> oleh armada DLH.<br><br><em>Tips:</em> Letakkan sampah di wadah tertutup di depan pagar sebelum pukul 06.45 WIB.',
    },
    minyak: {
        userText: 'Apa yang harus dilakukan dengan minyak jelantah sisa dapur?',
        functionCall: "cekKategoriSampah('minyak jelantah') -> cariMitraPengolah('biodiesel')",
        reply: 'Minyak jelantah masuk kategori <strong>limbah khusus B3</strong> 🛑.<br><br><strong>Peringatan:</strong> Jangan dibuang ke wastafel cuci piring karena menyumbat saluran pipa dan mencemari ekosistem sungai!<br><br><strong>Solusi:</strong> Saring remah makanan, dinginkan, tampung dalam botol tertutup, lalu jual ke Bank Sampah mitra pengolah biodiesel seharga Rp 4.000 - Rp 6.000/liter!',
    },
};

function sendScenario(key) {
    const scenario = presetScenarios[key];
    if (!scenario) return;

    chatMessages.value.push({
        sender: 'user',
        text: scenario.userText,
        functionCall: null,
    });

    isTyping.value = true;
    setTimeout(() => {
        isTyping.value = false;
        chatMessages.value.push({
            sender: 'bot',
            text: scenario.reply,
            functionCall: scenario.functionCall,
        });
    }, 750);
}

function handleCustomSubmit() {
    const text = userInput.value.trim();
    if (!text) return;
    userInput.value = '';

    chatMessages.value.push({
        sender: 'user',
        text: text,
        functionCall: null,
    });

    isTyping.value = true;
    setTimeout(() => {
        isTyping.value = false;
        const lower = text.toLowerCase();
        let reply = '';
        let func = '';

        if (lower.includes('baterai') || lower.includes('aki') || lower.includes('b3')) {
            reply = presetScenarios.baterai.reply;
            func = presetScenarios.baterai.functionCall;
        } else if (lower.includes('lokasi') || lower.includes('bank sampah') || lower.includes('tps')) {
            reply = presetScenarios.lokasi.reply;
            func = presetScenarios.lokasi.functionCall;
        } else if (lower.includes('jadwal') || lower.includes('angkut')) {
            reply = presetScenarios.jadwal.reply;
            func = presetScenarios.jadwal.functionCall;
        } else if (lower.includes('minyak') || lower.includes('jelantah')) {
            reply = presetScenarios.minyak.reply;
            func = presetScenarios.minyak.functionCall;
        } else {
            reply = `Pertanyaan Anda mengenai <em>"${text}"</em> telah diterima oleh PilahAI.<br><br>Dalam versi penuh Pilahki, Gemini API dengan <strong>Function Calling</strong> akan langsung mencocokkan kategori sampah, mencari Bank Sampah terdekat, atau mengecek jadwal angkut di wilayah Anda secara otonom!`;
            func = `geminiAssistantHub(query: '${text}')`;
        }

        chatMessages.value.push({
            sender: 'bot',
            text: reply,
            functionCall: func,
        });
    }, 850);
}

function askAboutWaste(item) {
    const el = document.getElementById('pilahai-demo');
    if (el) {
        el.scrollIntoView({ behavior: 'smooth' });
    }
    setTimeout(() => {
        userInput.value = `Bagaimana cara memilah dan menyalurkan ${item.name}?`;
        handleCustomSubmit();
    }, 400);
}

// FAQ Accordion State
const activeFaq = ref(null);
function toggleFaq(index) {
    activeFaq.value = activeFaq.value === index ? null : index;
}

const faqs = [
    {
        q: 'Apakah warga harus menginstal aplikasi di ponsel?',
        a: 'Tidak perlu. Pilahki berbasis web responsif yang sangat ringan. Warga cukup membuka tautan website melalui browser ponsel atau laptop kapan saja tanpa memakan ruang penyimpanan perangkat.',
    },
    {
        q: 'Bagaimana cara kerja chatbot PilahAI?',
        a: 'PilahAI terintegrasi dengan Gemini API bertenaga Function Calling. Saat Anda bertanya, AI memanggil fungsi internal Pilahki (cek kategori, cari lokasi fasilitas, cek jadwal angkut) dan mengembalikan respons yang akurat dalam satu sesi chat.',
    },
    {
        q: 'Apakah ada biaya untuk menggunakan platform Pilahki?',
        a: 'Pilahki 100% gratis untuk seluruh warga sebagai bentuk kontribusi peningkatan literasi lingkungan dan penataan kebersihan wilayah perkotaan.',
    },
    {
        q: 'Apa rencana pengembangan setelah landing page ini?',
        a: 'Proyek ini sedang dibangun menggunakan arsitektur Laravel 13 + Vue 3. Fitur selanjutnya meliputi otentikasi akun warga (riwayat setoran sampah & notifikasi jadwal wilayah) dan Dashboard Admin untuk mengelola data fasilitas serta edukasi.',
    },
];
</script>

<template>
    <Head title="Pilahki — Platform Cerdas Pemilahan Sampah Terintegrasi" />

    <div class="min-h-screen bg-slate-50 text-slate-800 antialiased selection:bg-emerald-500 selection:text-white flex flex-col font-sans">
        
        <!-- =========================================================
             1. STICKY NAVBAR
        ========================================================= -->
        <header class="fixed top-0 left-0 right-0 z-50 bg-white/90 backdrop-blur-md border-b border-slate-200/70 transition-all">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex items-center justify-between h-20">
                    
                    <!-- Brand Logo -->
                    <a href="#" class="flex items-center gap-2.5 group">
                        <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-emerald-600 to-teal-500 flex items-center justify-center text-white shadow-md shadow-emerald-600/20 group-hover:scale-105 transition-transform font-black">
                            🌱
                        </div>
                        <div class="flex flex-col">
                            <span class="text-2xl font-extrabold tracking-tight text-slate-900 flex items-center gap-1">
                                Pilah<span class="text-emerald-600">ki</span>
                                <span class="inline-block w-1.5 h-1.5 rounded-full bg-emerald-500"></span>
                            </span>
                            <span class="text-[10px] font-bold text-slate-400 uppercase tracking-wider -mt-1">Pilah Sampah Jadi Berkah</span>
                        </div>
                    </a>

                    <!-- Desktop Nav Links -->
                    <nav class="hidden md:flex items-center gap-7 text-sm font-semibold text-slate-600">
                        <a href="#masalah" class="hover:text-emerald-600 transition-colors">Masalah & Solusi</a>
                        <a href="#fitur" class="hover:text-emerald-600 transition-colors">5 Fitur Utama</a>
                        <a href="#cek-sampah" class="hover:text-emerald-600 transition-colors">Cek Sampah</a>
                        <a href="#pilahai-demo" class="hover:text-emerald-600 transition-colors flex items-center gap-1.5 text-emerald-700">
                            <span class="relative flex h-2 w-2">
                                <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75"></span>
                                <span class="relative inline-flex rounded-full h-2 w-2 bg-emerald-500"></span>
                            </span>
                            PilahAI Demo
                        </a>
                        <a href="#sdg-impact" class="hover:text-emerald-600 transition-colors">Dampak SDG</a>
                        <a href="#faq" class="hover:text-emerald-600 transition-colors">FAQ</a>
                    </nav>

                    <!-- Auth Actions -->
                    <div class="hidden lg:flex items-center gap-3">
                        <template v-if="canLogin">
                            <Link
                                v-if="$page.props.auth?.user"
                                :href="route('dashboard')"
                                class="px-5 py-2.5 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white font-bold text-sm shadow-md shadow-emerald-600/20 transition-all"
                            >
                                Dashboard
                            </Link>
                            <template v-else>
                                <Link
                                    :href="route('login')"
                                    class="px-4 py-2 text-sm font-bold text-slate-700 hover:text-emerald-600 transition-colors"
                                >
                                    Masuk
                                </Link>
                                <Link
                                    v-if="canRegister"
                                    :href="route('register')"
                                    class="px-5 py-2.5 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white font-bold text-sm shadow-md shadow-emerald-600/20 hover:shadow-lg transition-all"
                                >
                                    Daftar Akun
                                </Link>
                            </template>
                        </template>
                    </div>

                    <!-- Mobile Menu Button -->
                    <div class="flex items-center md:hidden">
                        <button
                            @click="mobileMenuOpen = !mobileMenuOpen"
                            class="p-2 rounded-xl text-slate-600 hover:text-emerald-600 hover:bg-slate-100 focus:outline-none"
                            aria-label="Toggle Menu"
                        >
                            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path v-if="!mobileMenuOpen" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
                                <path v-else stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                            </svg>
                        </button>
                    </div>

                </div>
            </div>

            <!-- Mobile Dropdown -->
            <div v-show="mobileMenuOpen" class="md:hidden border-t border-slate-200 bg-white px-4 pt-3 pb-6 space-y-2 shadow-xl">
                <a @click="mobileMenuOpen = false" href="#masalah" class="block px-3 py-2 rounded-lg text-sm font-semibold text-slate-700 hover:bg-emerald-50 hover:text-emerald-700">Masalah & Solusi</a>
                <a @click="mobileMenuOpen = false" href="#fitur" class="block px-3 py-2 rounded-lg text-sm font-semibold text-slate-700 hover:bg-emerald-50 hover:text-emerald-700">5 Fitur Utama</a>
                <a @click="mobileMenuOpen = false" href="#cek-sampah" class="block px-3 py-2 rounded-lg text-sm font-semibold text-slate-700 hover:bg-emerald-50 hover:text-emerald-700">Cek Sampah Cepat</a>
                <a @click="mobileMenuOpen = false" href="#pilahai-demo" class="block px-3 py-2 rounded-lg text-sm font-bold text-emerald-700 bg-emerald-50">PilahAI Demo</a>
                <a @click="mobileMenuOpen = false" href="#sdg-impact" class="block px-3 py-2 rounded-lg text-sm font-semibold text-slate-700 hover:bg-emerald-50 hover:text-emerald-700">Dampak SDG</a>
                <a @click="mobileMenuOpen = false" href="#faq" class="block px-3 py-2 rounded-lg text-sm font-semibold text-slate-700 hover:bg-emerald-50 hover:text-emerald-700">FAQ</a>
                <div class="pt-3 border-t border-slate-100 flex flex-col gap-2">
                    <template v-if="canLogin">
                        <Link v-if="$page.props.auth?.user" :href="route('dashboard')" class="w-full text-center py-2.5 rounded-xl bg-emerald-600 text-white font-bold text-sm">
                            Buka Dashboard
                        </Link>
                        <template v-else>
                            <Link :href="route('login')" class="w-full text-center py-2.5 rounded-xl bg-slate-100 text-slate-800 font-bold text-sm">
                                Masuk
                            </Link>
                            <Link v-if="canRegister" :href="route('register')" class="w-full text-center py-2.5 rounded-xl bg-emerald-600 text-white font-bold text-sm">
                                Buat Akun Baru
                            </Link>
                        </template>
                    </template>
                </div>
            </div>
        </header>

        <!-- =========================================================
             2. HERO SECTION
        ========================================================= -->
        <main class="flex-grow pt-20">
            <section class="relative overflow-hidden py-16 sm:py-24 bg-gradient-to-b from-emerald-50/50 via-white to-slate-50">
                <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                    <div class="grid grid-cols-1 lg:grid-cols-12 gap-12 items-center">
                        
                        <!-- Left Copy -->
                        <div class="lg:col-span-7 space-y-6 text-center lg:text-left">
                            <div class="inline-flex items-center gap-2 px-3.5 py-1.5 rounded-full bg-emerald-100/80 border border-emerald-300/60 text-emerald-800 text-xs font-semibold">
                                <span class="w-2 h-2 rounded-full bg-emerald-600 animate-pulse"></span>
                                HM TIF UNISSULA • Web Sustainability Competition
                            </div>

                            <h1 class="text-4xl sm:text-5xl lg:text-6xl font-black text-slate-900 tracking-tight leading-[1.15]">
                                Satu Platform Cerdas untuk <span class="text-transparent bg-clip-text bg-gradient-to-r from-emerald-600 to-teal-600">Pilah, Salurkan & Kelola</span> Sampah.
                            </h1>

                            <p class="text-base sm:text-lg text-slate-600 max-w-2xl mx-auto lg:mx-0 leading-relaxed">
                                <strong>Pilahki</strong> membantu warga mengenali kategori sampah dengan benar, menemukan Bank Sampah & TPS terdekat, melihat jadwal angkut, dan berkonsultasi langsung dengan asisten pintar <strong>PilahAI</strong>.
                            </p>

                            <div class="flex flex-col sm:flex-row items-center justify-center lg:justify-start gap-3.5 pt-2">
                                <a href="#cek-sampah" class="w-full sm:w-auto px-6 py-3.5 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white font-bold text-base shadow-lg shadow-emerald-600/25 transition-all text-center">
                                    🔍 Cek Jenis Sampahmu
                                </a>
                                <a href="#pilahai-demo" class="w-full sm:w-auto px-6 py-3.5 rounded-xl bg-white hover:bg-slate-50 text-slate-800 border border-slate-300 font-bold text-base shadow-sm transition-all text-center">
                                    🤖 Tanya PilahAI (Live Demo)
                                </a>
                            </div>

                            <div class="pt-6 grid grid-cols-3 gap-4 max-w-lg mx-auto lg:mx-0 border-t border-slate-200/80">
                                <div>
                                    <p class="text-2xl sm:text-3xl font-extrabold text-slate-900">4 Jenis</p>
                                    <p class="text-xs text-slate-500 font-medium">Standar Pemilahan</p>
                                </div>
                                <div class="border-x border-slate-200 px-3">
                                    <p class="text-2xl sm:text-3xl font-extrabold text-emerald-600">PilahAI</p>
                                    <p class="text-xs text-slate-500 font-medium">Asisten Gemini AI</p>
                                </div>
                                <div>
                                    <p class="text-2xl sm:text-3xl font-extrabold text-slate-900">3 Pilar</p>
                                    <p class="text-xs text-slate-500 font-medium">SDG 11, 13 & 4</p>
                                </div>
                            </div>
                        </div>

                        <!-- Right Visual Card -->
                        <div class="lg:col-span-5 relative">
                            <div class="relative mx-auto max-w-md bg-white rounded-3xl p-6 shadow-2xl border border-slate-100">
                                <div class="flex items-center justify-between pb-4 mb-4 border-b border-slate-100">
                                    <div class="flex items-center gap-3">
                                        <div class="w-10 h-10 rounded-xl bg-emerald-100 text-emerald-700 flex items-center justify-center font-bold text-lg">
                                            🌿
                                        </div>
                                        <div>
                                            <h3 class="font-bold text-slate-900 text-sm">Pilahki Smart Hub</h3>
                                            <p class="text-[11px] text-emerald-600 font-semibold">Gemini Function Calling Ready</p>
                                        </div>
                                    </div>
                                    <span class="flex h-2.5 w-2.5 rounded-full bg-emerald-500"></span>
                                </div>

                                <div class="space-y-3.5 text-xs">
                                    <div class="flex justify-end">
                                        <div class="bg-emerald-600 text-white p-3 rounded-2xl rounded-tr-none max-w-[85%] leading-relaxed shadow-sm">
                                            Kardus bekas dan botol plastik saya bawa ke mana ya?
                                        </div>
                                    </div>
                                    <div class="flex gap-2.5">
                                        <div class="w-7 h-7 rounded-full bg-slate-900 text-white flex items-center justify-center text-[10px] font-bold shrink-0 mt-1">
                                            AI
                                        </div>
                                        <div class="bg-slate-50 border border-slate-200 text-slate-800 p-3 rounded-2xl rounded-tl-none leading-relaxed shadow-sm">
                                            <div class="mb-1 text-[10px] text-emerald-700 font-mono bg-emerald-50 px-1.5 py-0.5 rounded border border-emerald-200 w-fit">
                                                ✓ Kategori: Anorganik Bernilai Ekonomis
                                            </div>
                                            Setorkan ke <strong>Bank Sampah Berkah Resik</strong> (± 700m dari Anda). Buka hari ini sampai pukul 12.00 WIB!
                                        </div>
                                    </div>

                                    <div class="pt-2 grid grid-cols-2 gap-2">
                                        <div class="p-2.5 rounded-xl bg-emerald-50 border border-emerald-100 flex items-center gap-2">
                                            <span class="text-base">📍</span>
                                            <div>
                                                <p class="text-[11px] font-bold text-slate-800">Bank Sampah</p>
                                                <p class="text-[10px] text-slate-500">700 m terdekat</p>
                                            </div>
                                        </div>
                                        <div class="p-2.5 rounded-xl bg-teal-50 border border-teal-100 flex items-center gap-2">
                                            <span class="text-base">🗓️</span>
                                            <div>
                                                <p class="text-[11px] font-bold text-slate-800">Jadwal Angkut</p>
                                                <p class="text-[10px] text-slate-500">Besok 07.00 WIB</p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>

                    </div>
                </div>
            </section>

            <!-- =========================================================
                 3. MASALAH & SOLUSI
            ========================================================= -->
            <section id="masalah" class="py-16 bg-white border-y border-slate-200">
                <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                    <div class="text-center max-w-3xl mx-auto mb-14">
                        <span class="text-xs font-bold tracking-wider uppercase text-emerald-600 bg-emerald-50 px-3 py-1 rounded-full border border-emerald-200">
                            Tantangan Nyata
                        </span>
                        <h2 class="text-3xl sm:text-4xl font-extrabold text-slate-900 tracking-tight mt-3">
                            Mengapa Sampah Rumah Tangga Kerap Jadi Masalah?
                        </h2>
                        <p class="text-base text-slate-600 mt-2">
                            Banyak warga ingin menjaga kelestarian lingkungan, namun terkendala informasi yang tersebar dan tidak terpusat.
                        </p>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                        <div class="bg-slate-50 rounded-2xl p-6 border border-slate-200 hover:border-emerald-300 transition-all">
                            <div class="text-3xl mb-3">❓</div>
                            <h3 class="text-lg font-bold text-slate-900 mb-2">Bingung Cara Memilah</h3>
                            <p class="text-xs text-slate-600 leading-relaxed">
                                Sulit membedakan sampah organik, daur ulang anorganik, limbah B3, atau residu yang harus ke TPA.
                            </p>
                            <p class="mt-4 text-xs font-bold text-emerald-700">✓ Solusi: Fitur Pilah Sampah (7.1)</p>
                        </div>

                        <div class="bg-slate-50 rounded-2xl p-6 border border-slate-200 hover:border-emerald-300 transition-all">
                            <div class="text-3xl mb-3">🗺️</div>
                            <h3 class="text-lg font-bold text-slate-900 mb-2">Sulit Cari Lokasi</h3>
                            <p class="text-xs text-slate-600 leading-relaxed">
                                Tidak tahu lokasi Bank Sampah atau TPS3R terdekat, serta jam operasional dan sampah yang diterima.
                            </p>
                            <p class="mt-4 text-xs font-bold text-emerald-700">✓ Solusi: Fitur Cari Lokasi (7.2)</p>
                        </div>

                        <div class="bg-slate-50 rounded-2xl p-6 border border-slate-200 hover:border-emerald-300 transition-all">
                            <div class="text-3xl mb-3">⏰</div>
                            <h3 class="text-lg font-bold text-slate-900 mb-2">Jadwal Angkut Gaib</h3>
                            <p class="text-xs text-slate-600 leading-relaxed">
                                Sering terlambat membuang sampah hingga menumpuk dan menimbulkan bau di depan rumah.
                            </p>
                            <p class="mt-4 text-xs font-bold text-emerald-700">✓ Solusi: Jadwal Angkut Wilayah (7.3)</p>
                        </div>

                        <div class="bg-slate-50 rounded-2xl p-6 border border-slate-200 hover:border-emerald-300 transition-all">
                            <div class="text-3xl mb-3">🔄</div>
                            <h3 class="text-lg font-bold text-slate-900 mb-2">Bingung Mau Disalurkan ke Mana</h3>
                            <p class="text-xs text-slate-600 leading-relaxed">
                                Sampah yang sudah dipilah akhirnya tercampur kembali karena tidak tahu ke mana harus menyalurkannya.
                            </p>
                            <p class="mt-4 text-xs font-bold text-emerald-700">✓ Solusi: Asisten Percakapan PilahAI (7.5)</p>
                        </div>
                    </div>
                </div>
            </section>

            <!-- =========================================================
                 4. FITUR UTAMA DARI PRD
            ========================================================= -->
            <section id="fitur" class="py-20 bg-slate-50">
                <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                    <div class="text-center max-w-3xl mx-auto mb-16">
                        <span class="text-xs font-bold tracking-wider uppercase text-emerald-600 bg-emerald-100/70 px-3 py-1 rounded-full border border-emerald-200">
                            Fitur Terpadu
                        </span>
                        <h2 class="text-3xl sm:text-4xl font-extrabold text-slate-900 tracking-tight mt-3">
                            5 Pilar Fitur Utama Pilahki
                        </h2>
                        <p class="text-base text-slate-600 mt-2">
                            Disusun sesuai spesifikasi [PRD.md] untuk kompetisi lomba web keberlanjutan HM TIF UNISSULA.
                        </p>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                        <!-- Fitur 1 -->
                        <div class="bg-white rounded-3xl p-7 border border-slate-200 shadow-sm hover:shadow-lg transition-all flex flex-col justify-between">
                            <div>
                                <span class="text-xs font-bold text-emerald-600 uppercase tracking-wider">Fitur 7.1</span>
                                <h3 class="text-xl font-bold text-slate-900 mt-1 mb-2">Pilah Sampah</h3>
                                <p class="text-xs text-slate-600 leading-relaxed mb-4">
                                    Pencarian dan pengenalan kategori sampah dalam maksimal 3 langkah dengan instruksi penanganan praktis untuk 4 kategori (Organik, Anorganik, B3, Residu).
                                </p>
                            </div>
                            <a href="#cek-sampah" class="text-xs font-bold text-emerald-700 hover:underline">Coba Pencarian Cepat &rarr;</a>
                        </div>

                        <!-- Fitur 2 -->
                        <div class="bg-white rounded-3xl p-7 border border-slate-200 shadow-sm hover:shadow-lg transition-all flex flex-col justify-between">
                            <div>
                                <span class="text-xs font-bold text-blue-600 uppercase tracking-wider">Fitur 7.2</span>
                                <h3 class="text-xl font-bold text-slate-900 mt-1 mb-2">Cari Lokasi Fasilitas</h3>
                                <p class="text-xs text-slate-600 leading-relaxed mb-4">
                                    Menemukan Bank Sampah dan TPS/TPS3R terdekat dengan informasi jam operasional, jenis sampah yang diterima, dan estimasi jarak dari lokasi warga.
                                </p>
                            </div>
                            <span class="text-xs text-slate-400">Data demo siap presentasi</span>
                        </div>

                        <!-- Fitur 3 -->
                        <div class="bg-white rounded-3xl p-7 border border-slate-200 shadow-sm hover:shadow-lg transition-all flex flex-col justify-between">
                            <div>
                                <span class="text-xs font-bold text-teal-600 uppercase tracking-wider">Fitur 7.3</span>
                                <h3 class="text-xl font-bold text-slate-900 mt-1 mb-2">Jadwal Angkut</h3>
                                <p class="text-xs text-slate-600 leading-relaxed mb-4">
                                    Informasi jadwal penjemputan sampah per wilayah dan jenis sampah, membantu warga menyiapkan wadah sampah tepat waktu sebelum armada tiba.
                                </p>
                            </div>
                            <span class="text-xs text-slate-400">Tampilan kalender ringkas</span>
                        </div>

                        <!-- Fitur 4 -->
                        <div class="bg-white rounded-3xl p-7 border border-slate-200 shadow-sm hover:shadow-lg transition-all flex flex-col justify-between">
                            <div>
                                <span class="text-xs font-bold text-amber-600 uppercase tracking-wider">Fitur 7.4</span>
                                <h3 class="text-xl font-bold text-slate-900 mt-1 mb-2">Panduan Literasi</h3>
                                <p class="text-xs text-slate-600 leading-relaxed mb-4">
                                    Modul edukasi ramah keluarga seputar pembuatan kompos mandiri, daur ulang kreatif, dan penanganan limbah B3 menggunakan bahasa awam.
                                </p>
                            </div>
                            <span class="text-xs text-slate-400">Mendukung pilar SDG 4</span>
                        </div>

                        <!-- Fitur 5: PilahAI Hub -->
                        <div class="md:col-span-2 bg-gradient-to-br from-slate-900 via-slate-800 to-emerald-950 text-white rounded-3xl p-8 shadow-xl border border-emerald-500/30 flex flex-col justify-between">
                            <div>
                                <div class="flex items-center justify-between mb-4">
                                    <span class="text-xs font-bold text-emerald-400 uppercase tracking-wider">Fitur 7.5 (Pusat Hub)</span>
                                    <span class="px-3 py-1 rounded-full text-[11px] font-mono font-bold bg-emerald-500/20 text-emerald-300 border border-emerald-500/40">
                                        Gemini API Function Calling
                                    </span>
                                </div>
                                <h3 class="text-2xl font-black text-white mb-2">PilahAI: Asisten Cerdas Satu Titik Temu</h3>
                                <p class="text-xs sm:text-sm text-slate-300 leading-relaxed mb-4">
                                    PilahAI menghubungkan semua fitur dalam satu antarmuka percakapan bebas. Model dapat memanggil fungsi internal Pilahki untuk memeriksa kategori sampah, mencari bank sampah terdekat, hingga mengecek jadwal angkut tanpa berpindah halaman.
                                </p>
                            </div>
                            <a href="#pilahai-demo" class="w-fit px-5 py-2.5 rounded-xl bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold text-xs transition-colors">
                                Coba Simulasi PilahAI &rarr;
                            </a>
                        </div>
                    </div>
                </div>
            </section>

            <!-- =========================================================
                 5. INTERACTIVE LIVE CHECKER (20+ ITEMS DATABASE)
            ========================================================= -->
            <section id="cek-sampah" class="py-20 bg-white border-t border-slate-200">
                <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                    <div class="text-center max-w-3xl mx-auto mb-10">
                        <span class="text-xs font-bold tracking-wider uppercase text-emerald-600 bg-emerald-50 px-3 py-1 rounded-full border border-emerald-200">
                            Simulasi Interaktif
                        </span>
                        <h2 class="text-3xl sm:text-4xl font-extrabold text-slate-900 tracking-tight mt-3">
                            Cek Jenis Sampah & Cara Penanganannya
                        </h2>
                        <p class="text-sm text-slate-600 mt-2">
                            Database 20+ jenis sampah rumah tangga umum siap cari secara instan.
                        </p>
                    </div>

                    <!-- Search & Filters -->
                    <div class="max-w-4xl mx-auto mb-8 space-y-4">
                        <div class="relative">
                            <input
                                v-model="searchQuery"
                                type="text"
                                placeholder="Ketik jenis sampah (cth: kulit pisang, kardus, baterai, minyak jelantah, popok...)"
                                class="w-full px-5 py-3.5 bg-slate-50 border border-slate-200 rounded-2xl text-slate-800 text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:bg-white transition-all shadow-inner"
                            />
                        </div>

                        <!-- Chips -->
                        <div class="flex flex-wrap items-center gap-2 text-xs">
                            <span class="text-slate-500 font-medium">Contoh cepat:</span>
                            <button @click="setQuery('Kulit Buah')" class="px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-emerald-100 text-slate-700">Kulit Buah & Pisang</button>
                            <button @click="setQuery('Botol Plastik')" class="px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-emerald-100 text-slate-700">Botol Plastik PET</button>
                            <button @click="setQuery('Baterai Bekas')" class="px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-emerald-100 text-slate-700">Baterai Bekas</button>
                            <button @click="setQuery('Minyak Jelantah')" class="px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-emerald-100 text-slate-700">Minyak Jelantah</button>
                            <button @click="setQuery('Popok Bayi')" class="px-2.5 py-1 rounded-lg bg-slate-100 hover:bg-emerald-100 text-slate-700">Popok Sekali Pakai</button>
                        </div>

                        <!-- Category Filters -->
                        <div class="flex flex-wrap items-center justify-between gap-3 pt-2">
                            <div class="flex flex-wrap gap-2">
                                <button
                                    @click="selectedCategory = 'all'"
                                    :class="selectedCategory === 'all' ? 'bg-emerald-600 text-white shadow-sm' : 'bg-white text-slate-600 border border-slate-200 hover:bg-slate-50'"
                                    class="px-4 py-1.5 rounded-xl text-xs font-bold transition-all"
                                >
                                    Semua
                                </button>
                                <button
                                    @click="selectedCategory = 'organik'"
                                    :class="selectedCategory === 'organik' ? 'bg-emerald-600 text-white shadow-sm' : 'bg-white text-slate-600 border border-slate-200 hover:bg-slate-50'"
                                    class="px-4 py-1.5 rounded-xl text-xs font-bold transition-all"
                                >
                                    🌱 Organik
                                </button>
                                <button
                                    @click="selectedCategory = 'anorganik'"
                                    :class="selectedCategory === 'anorganik' ? 'bg-emerald-600 text-white shadow-sm' : 'bg-white text-slate-600 border border-slate-200 hover:bg-slate-50'"
                                    class="px-4 py-1.5 rounded-xl text-xs font-bold transition-all"
                                >
                                    ♻️ Anorganik
                                </button>
                                <button
                                    @click="selectedCategory = 'b3'"
                                    :class="selectedCategory === 'b3' ? 'bg-emerald-600 text-white shadow-sm' : 'bg-white text-slate-600 border border-slate-200 hover:bg-slate-50'"
                                    class="px-4 py-1.5 rounded-xl text-xs font-bold transition-all"
                                >
                                    ⚠️ Limbah B3
                                </button>
                                <button
                                    @click="selectedCategory = 'residu'"
                                    :class="selectedCategory === 'residu' ? 'bg-emerald-600 text-white shadow-sm' : 'bg-white text-slate-600 border border-slate-200 hover:bg-slate-50'"
                                    class="px-4 py-1.5 rounded-xl text-xs font-bold transition-all"
                                >
                                    🗑️ Residu
                                </button>
                            </div>
                            <span class="text-xs text-slate-500 font-medium">{{ filteredWaste.length }} jenis sampah ditemukan</span>
                        </div>
                    </div>

                    <!-- Results Grid -->
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-5 max-w-5xl mx-auto">
                        <div
                            v-for="item in filteredWaste"
                            :key="item.id"
                            class="bg-white rounded-2xl p-5 border border-slate-100 shadow-sm hover:shadow-md hover:border-emerald-200 transition-all flex flex-col justify-between"
                        >
                            <div>
                                <div class="flex items-start justify-between gap-3 mb-2">
                                    <h4 class="font-bold text-slate-900 text-sm">{{ item.name }}</h4>
                                    <span :class="item.badgeColor" class="px-2.5 py-0.5 rounded-full text-[11px] font-bold border">
                                        {{ item.category }}
                                    </span>
                                </div>
                                <p class="text-xs text-slate-600 leading-relaxed">{{ item.handling }}</p>
                            </div>
                            <div class="mt-4 pt-3 border-t border-slate-100 flex items-center justify-between text-xs">
                                <span class="text-slate-400">PRD 7.1 Terverifikasi</span>
                                <button @click="askAboutWaste(item)" class="text-emerald-700 font-bold hover:underline">
                                    Tanya AI &rarr;
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- =========================================================
                 6. PILAHAI SIMULATOR (GEMINI FUNCTION CALLING)
            ========================================================= -->
            <section id="pilahai-demo" class="py-20 bg-gradient-to-b from-slate-900 to-slate-950 text-white">
                <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
                    <div class="text-center max-w-3xl mx-auto mb-10">
                        <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-emerald-500/20 text-emerald-300 text-xs font-mono font-semibold border border-emerald-500/30 mb-3">
                            <span class="w-2 h-2 rounded-full bg-emerald-400 animate-ping"></span>
                            Simulasi Gemini API Function Calling
                        </div>
                        <h2 class="text-3xl sm:text-4xl font-extrabold text-white tracking-tight">
                            Uji Coba Kecerdasan PilahAI
                        </h2>
                        <p class="text-xs sm:text-sm text-slate-300 mt-2">
                            Klik skenario berikut untuk melihat bagaimana PilahAI mengeksekusi integrasi fungsi internal:
                        </p>
                    </div>

                    <!-- Quick Scenarios -->
                    <div class="flex flex-wrap items-center justify-center gap-2 mb-6 text-xs">
                        <button @click="sendScenario('baterai')" class="px-3.5 py-2 rounded-xl bg-slate-800 hover:bg-emerald-900/60 border border-slate-700 text-slate-200 transition-all font-semibold">
                            🔋 Tanya Limbah B3 Baterai
                        </button>
                        <button @click="sendScenario('lokasi')" class="px-3.5 py-2 rounded-xl bg-slate-800 hover:bg-emerald-900/60 border border-slate-700 text-slate-200 transition-all font-semibold">
                            📍 Cari Bank Sampah Terdekat
                        </button>
                        <button @click="sendScenario('jadwal')" class="px-3.5 py-2 rounded-xl bg-slate-800 hover:bg-emerald-900/60 border border-slate-700 text-slate-200 transition-all font-semibold">
                            🗓️ Jadwal Angkut Organik
                        </button>
                        <button @click="sendScenario('minyak')" class="px-3.5 py-2 rounded-xl bg-slate-800 hover:bg-emerald-900/60 border border-slate-700 text-slate-200 transition-all font-semibold">
                            🍳 Buang Minyak Jelantah
                        </button>
                    </div>

                    <!-- Chat Container Box -->
                    <div class="bg-slate-900 rounded-3xl border border-slate-800 shadow-2xl overflow-hidden flex flex-col h-[500px]">
                        <div class="px-6 py-4 bg-slate-800/80 border-b border-slate-700 flex items-center justify-between">
                            <div class="flex items-center gap-3">
                                <div class="w-8 h-8 rounded-lg bg-emerald-500 text-slate-950 flex items-center justify-center font-bold">
                                    AI
                                </div>
                                <div>
                                    <h4 class="font-bold text-white text-xs sm:text-sm">PilahAI Assistant Hub</h4>
                                    <p class="text-[10px] text-slate-400">Gemini Pro/Flash API Simulator</p>
                                </div>
                            </div>
                            <span class="text-xs text-emerald-400 font-semibold flex items-center gap-1">
                                <span class="w-2 h-2 rounded-full bg-emerald-500 animate-pulse"></span> Online
                            </span>
                        </div>

                        <!-- Chat Messages -->
                        <div class="flex-1 p-5 overflow-y-auto space-y-4">
                            <div
                                v-for="(msg, i) in chatMessages"
                                :key="i"
                                :class="msg.sender === 'user' ? 'justify-end' : 'justify-start'"
                                class="flex gap-2.5"
                            >
                                <div v-if="msg.sender === 'bot'" class="w-7 h-7 rounded-full bg-emerald-600 text-white flex items-center justify-center text-[10px] font-bold shrink-0 mt-1">
                                    AI
                                </div>
                                <div
                                    :class="msg.sender === 'user' ? 'bg-emerald-600 text-white rounded-tr-none' : 'bg-slate-50 text-slate-800 rounded-tl-none'"
                                    class="max-w-[85%] sm:max-w-[75%] p-3.5 rounded-2xl text-xs sm:text-sm leading-relaxed shadow-sm"
                                >
                                    <div v-if="msg.functionCall" class="mb-2 text-[10px] font-mono text-emerald-800 bg-emerald-50 px-2 py-1 rounded border border-emerald-200">
                                        ⚡ Function Call: <code>{{ msg.functionCall }}</code>
                                    </div>
                                    <div v-html="msg.text"></div>
                                </div>
                            </div>

                            <!-- Typing Indicator -->
                            <div v-if="isTyping" class="flex gap-2.5 justify-start items-center">
                                <div class="w-7 h-7 rounded-full bg-emerald-600 text-white flex items-center justify-center text-[10px] font-bold shrink-0">
                                    AI
                                </div>
                                <div class="bg-slate-800 text-slate-300 px-4 py-2.5 rounded-2xl text-xs">
                                    PilahAI sedang memanggil fungsi internal...
                                </div>
                            </div>
                        </div>

                        <!-- Input -->
                        <div class="p-4 bg-slate-800/80 border-t border-slate-700">
                            <form @submit.prevent="handleCustomSubmit" class="flex gap-2">
                                <input
                                    v-model="userInput"
                                    type="text"
                                    placeholder="Ketik pertanyaan Anda ke PilahAI di sini..."
                                    class="flex-1 bg-slate-900 border border-slate-700 rounded-xl px-4 py-2.5 text-xs sm:text-sm text-white focus:outline-none focus:border-emerald-500"
                                />
                                <button type="submit" class="px-5 py-2.5 rounded-xl bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-bold text-xs sm:text-sm transition-colors">
                                    Kirim
                                </button>
                            </form>
                        </div>
                    </div>
                </div>
            </section>

            <!-- =========================================================
                 7. DAMPAK KEBERLANJUTAN (SDG)
            ========================================================= -->
            <section id="sdg-impact" class="py-20 bg-white">
                <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                    <div class="text-center max-w-3xl mx-auto mb-14">
                        <span class="text-xs font-bold tracking-wider uppercase text-emerald-600 bg-emerald-50 px-3 py-1 rounded-full border border-emerald-200">
                            Pilar Keberlanjutan
                        </span>
                        <h2 class="text-3xl sm:text-4xl font-extrabold text-slate-900 tracking-tight mt-3">
                            Kontribusi Terhadap Sustainable Development Goals
                        </h2>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                        <div class="rounded-3xl p-7 bg-amber-50/60 border border-amber-200 flex flex-col justify-between">
                            <div>
                                <span class="px-3 py-1 rounded-full text-xs font-bold bg-amber-500 text-white">SDG 11</span>
                                <h3 class="text-lg font-bold text-slate-900 mt-4 mb-2">Kota & Permukiman Berkelanjutan</h3>
                                <p class="text-xs text-slate-600 leading-relaxed">
                                    Fitur Cari Lokasi dan Jadwal Angkut mendukung penataan sampah kota agar tidak menumpuk liar dan mencemari drainase permukiman.
                                </p>
                            </div>
                            <p class="mt-6 text-xs font-semibold text-amber-700">Target sanitasi perkotaan bersih</p>
                        </div>

                        <div class="rounded-3xl p-7 bg-emerald-50/60 border border-emerald-200 flex flex-col justify-between">
                            <div>
                                <span class="px-3 py-1 rounded-full text-xs font-bold bg-emerald-600 text-white">SDG 13</span>
                                <h3 class="text-lg font-bold text-slate-900 mt-4 mb-2">Penanganan Perubahan Iklim</h3>
                                <p class="text-xs text-slate-600 leading-relaxed">
                                    Pemilahan sampah organik langsung dari sumber menekan timbulan sampah di TPA, mengurangi emisi gas metana (CH₄) pemicu pemanasan global.
                                </p>
                            </div>
                            <p class="mt-6 text-xs font-semibold text-emerald-700">Pengurangan emisi rumah kaca</p>
                        </div>

                        <div class="rounded-3xl p-7 bg-blue-50/60 border border-blue-200 flex flex-col justify-between">
                            <div>
                                <span class="px-3 py-1 rounded-full text-xs font-bold bg-blue-600 text-white">SDG 4</span>
                                <h3 class="text-lg font-bold text-slate-900 mt-4 mb-2">Pendidikan Berkualitas</h3>
                                <p class="text-xs text-slate-600 leading-relaxed">
                                    Panduan edukasi dan interaksi PilahAI menumbuhkan literasi ekologis yang mudah dipahami seluruh kalangan warga.
                                </p>
                            </div>
                            <p class="mt-6 text-xs font-semibold text-blue-700">Literasi lingkungan inklusif</p>
                        </div>
                    </div>
                </div>
            </section>

            <!-- =========================================================
                 8. FAQ SECTION
            ========================================================= -->
            <section id="faq" class="py-20 bg-slate-50 border-t border-slate-200">
                <div class="max-w-3xl mx-auto px-4 sm:px-6 lg:px-8">
                    <div class="text-center mb-12">
                        <span class="text-xs font-bold tracking-wider uppercase text-emerald-600 bg-emerald-100/70 px-3 py-1 rounded-full">
                            Pertanyaan Umum
                        </span>
                        <h2 class="text-3xl font-extrabold text-slate-900 tracking-tight mt-3">
                            Frequently Asked Questions
                        </h2>
                    </div>

                    <div class="space-y-3">
                        <div
                            v-for="(faq, idx) in faqs"
                            :key="idx"
                            class="bg-white border border-slate-200 rounded-2xl overflow-hidden"
                        >
                            <button
                                @click="toggleFaq(idx)"
                                class="w-full px-6 py-4 text-left flex items-center justify-between font-bold text-slate-900 text-sm hover:text-emerald-700 transition-colors"
                            >
                                <span>{{ faq.q }}</span>
                                <span class="text-slate-400 font-normal text-lg">
                                    {{ activeFaq === idx ? '−' : '+' }}
                                </span>
                            </button>
                            <div
                                v-show="activeFaq === idx"
                                class="px-6 pb-4 text-xs sm:text-sm text-slate-600 leading-relaxed border-t border-slate-100 pt-3"
                            >
                                {{ faq.a }}
                            </div>
                        </div>
                    </div>
                </div>
            </section>
        </main>

        <!-- =========================================================
             9. FOOTER
        ========================================================= -->
        <footer class="bg-slate-950 text-slate-400 pt-16 pb-12 border-t border-slate-800 text-xs">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="grid grid-cols-1 md:grid-cols-4 gap-8 pb-10 border-b border-slate-800">
                    <div class="space-y-3">
                        <span class="text-xl font-extrabold text-white">Pilah<span class="text-emerald-500">ki</span></span>
                        <p class="text-slate-400 leading-relaxed">
                            Platform pengelolaan sampah terintegrasi berbasis Laravel {{ laravelVersion }} & Vue.
                        </p>
                    </div>
                    <div>
                        <h4 class="font-bold text-white mb-2">Menu Cepat</h4>
                        <ul class="space-y-1.5 text-slate-400">
                            <li><a href="#masalah" class="hover:text-emerald-400">Masalah & Solusi</a></li>
                            <li><a href="#fitur" class="hover:text-emerald-400">5 Fitur Utama</a></li>
                            <li><a href="#cek-sampah" class="hover:text-emerald-400">Cek Sampah</a></li>
                            <li><a href="#pilahai-demo" class="hover:text-emerald-400">PilahAI Demo</a></li>
                        </ul>
                    </div>
                    <div>
                        <h4 class="font-bold text-white mb-2">Kategori</h4>
                        <ul class="space-y-1.5 text-slate-400">
                            <li>Organik</li>
                            <li>Anorganik</li>
                            <li>Limbah B3</li>
                            <li>Residu (TPA)</li>
                        </ul>
                    </div>
                    <div>
                        <h4 class="font-bold text-white mb-2">Kompetisi</h4>
                        <p class="text-slate-400 leading-relaxed">
                            HM TIF UNISSULA 2026<br>Tema: Sustainability & Lingkungan Hidup.
                        </p>
                    </div>
                </div>
                <div class="pt-6 flex flex-col sm:flex-row justify-between items-center text-slate-500 gap-2">
                    <p>&copy; 2026 Pilahki. All rights reserved.</p>
                    <p>Laravel v{{ laravelVersion }} (PHP v{{ phpVersion }})</p>
                </div>
            </div>
        </footer>

    </div>
</template>
