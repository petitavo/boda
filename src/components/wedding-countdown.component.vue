<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue';
import gsap from 'gsap';

const isRevealed = ref(false);
const timeLeft = ref({
  dias: 0,
  horas: 0,
  minutos: 0,
  segundos: 0
});
const isPlaying = ref(false);
let audio: HTMLAudioElement | null = null;
const targetDate = new Date('2024-12-31');
let timer: number;

const calculateTimeLeft = () => {
  const difference = targetDate.getTime() - new Date().getTime();
  if (difference > 0) {
    timeLeft.value = {
      dias: Math.floor(difference / (1000 * 60 * 60 * 24)),
      horas: Math.floor((difference / (1000 * 60 * 60)) % 24),
      minutos: Math.floor((difference / 1000 / 60) % 60),
      segundos: Math.floor((difference / 1000) % 60)
    };
  }
};

const toggleMusic = () => {
  if (!audio) return;

  if (isPlaying.value) {
    audio.pause();
  } else {
    audio.play();
  }
  isPlaying.value = !isPlaying.value;
};

const revealInvitation = () => {
  isRevealed.value = true;
  gsap.to('.blur-overlay', {
    opacity: 0,
    duration: 1,
    onComplete: () => {
      if (audio) audio.play();
      isPlaying.value = true;
    }
  });

  // Animate title
  gsap.from('.title', {
    opacity: 0,
    y: -50,
    duration: 1.5,
    delay: 0.5,
    ease: 'back.out'
  });

  // Animate circles and flowers one by one
  const circles = document.querySelectorAll('.countdown-item');
  circles.forEach((circle, index) => {
    gsap.from(circle, {
      scale: 0,
      opacity: 0,
      duration: 0.8,
      delay: 1 + (index * 0.3),
      ease: 'back.out(1.7)'
    });

    // Animate flowers
    const flower = circle.querySelector('.flower');
    if (flower) {
      gsap.from(flower, {
        opacity: 0,
        scale: 0,
        rotation: -180,
        duration: 1,
        delay: 1.3 + (index * 0.3),
        ease: 'back.out(1.7)'
      });
    }
  });

  // Animate envelope
  gsap.from('.envelope', {
    opacity: 0,
    scale: 0,
    y: 100,
    duration: 1.5,
    delay: 2.5,
    ease: 'back.out(1.4)'
  });
};

onMounted(() => {
  calculateTimeLeft();
  timer = setInterval(calculateTimeLeft, 1000);
  audio = new Audio('../assets/img/badbunny.mp3');
  audio.loop = true;
});

onBeforeUnmount(() => {
  clearInterval(timer);
  if (audio) {
    audio.pause();
    audio = null;
  }
});
</script>

<template>
  <div class="wedding-app">
    <!-- Blur overlay -->
    <div v-if="!isRevealed" class="blur-overlay">
      <button @click="revealInvitation" class="reveal-button">
        Ver Invitación
      </button>
    </div>

    <div class="countdown-container" :class="{ revealed: isRevealed }">
      <!-- Controles de música -->
      <div v-if="isRevealed" class="music-controls">
        <button @click="toggleMusic" class="music-button">
          <span v-if="isPlaying" class="icon">⏸</span>
          <span v-else class="icon">▶️</span>
          <span class="music-note">🎵</span>
        </button>
      </div>

      <!-- Título -->
      <h1 class="title">UNO DE LOS DÍAS MÁS IMPORTANTES DE<br>NUESTRAS VIDAS ESTÁ POR LLEGAR, FALTAN...</h1>

      <!-- Círculos de cuenta regresiva -->
      <div class="countdown-circles">
        <div class="countdown-item">
          <div class="circle">
            <img src="../assets/img/F2-green.png" alt="Floral decoration" class="flower flower-1"   />
            <img src="../assets/img/F1-PURPLE.png" alt="Floral decoration" class="flower flower-2"  />
            <img src="../assets/img/F3-verde.png" alt="Floral decoration" class="flower flower-3"   />
            <img src="../assets/img/F4-2.png" alt="Floral decoration" class="flower flower-4"   />

            <img src="../assets/img/F2-green.png" alt="Floral decoration" class="flower flower-5" />
            <img src="../assets/img/F1-PURPLE.png" alt="Floral decoration" class="flower flower-6"  />
            <img src="../assets/img/F3-verde.png" alt="Floral decoration" class="flower flower-7"   />
            <img src="../assets/img/F4-2.png" alt="Floral decoration" class="flower flower-8"   />

            <span class="number">{{ timeLeft.dias }}</span>
            <span class="label">días</span>
          </div>
        </div>
        <div class="countdown-item">
          <div class="circle">
            <img src="../assets/img/F2-green.png" alt="Floral decoration" class="flower flower-1" />
            <img src="../assets/img/F1-PURPLE.png" alt="Floral decoration" class="flower flower-2"  />
            <img src="../assets/img/F3-verde.png" alt="Floral decoration" class="flower flower-3"   />
            <img src="../assets/img/F4-2.png" alt="Floral decoration" class="flower flower-4"   />

            <img src="../assets/img/F2-green.png" alt="Floral decoration" class="flower flower-5" />
            <img src="../assets/img/F1-PURPLE.png" alt="Floral decoration" class="flower flower-6"  />
            <img src="../assets/img/F3-verde.png" alt="Floral decoration" class="flower flower-7"   />
            <img src="../assets/img/F4-2.png" alt="Floral decoration" class="flower flower-8"   />

            <span class="number">{{ timeLeft.horas }}</span>
            <span class="label">horas</span>
          </div>
        </div>
        <div class="countdown-item">
          <div class="circle">
            <img src="../assets/img/F2-green.png" alt="Floral decoration" class="flower flower-1" />
            <img src="../assets/img/F1-PURPLE.png" alt="Floral decoration" class="flower flower-2"  />
            <img src="../assets/img/F3-verde.png" alt="Floral decoration" class="flower flower-3"   />
            <img src="../assets/img/F4-2.png" alt="Floral decoration" class="flower flower-4"   />

            <img src="../assets/img/F2-green.png" alt="Floral decoration" class="flower flower-5" />
            <img src="../assets/img/F1-PURPLE.png" alt="Floral decoration" class="flower flower-6"  />
            <img src="../assets/img/F3-verde.png" alt="Floral decoration" class="flower flower-7"   />
            <img src="../assets/img/F4-2.png" alt="Floral decoration" class="flower flower-8"   />
            <span class="number">{{ timeLeft.minutos }}</span>
            <span class="label">min</span>
          </div>
        </div>
        <div class="countdown-item">
          <div class="circle">
            <img src="../assets/img/F2-green.png" alt="Floral decoration" class="flower flower-1" />
            <img src="../assets/img/F1-PURPLE.png" alt="Floral decoration" class="flower flower-2"  />
            <img src="../assets/img/F3-verde.png" alt="Floral decoration" class="flower flower-3"   />
            <img src="../assets/img/F4-2.png" alt="Floral decoration" class="flower flower-4"   />

            <img src="../assets/img/F2-green.png" alt="Floral decoration" class="flower flower-5" />
            <img src="../assets/img/F1-PURPLE.png" alt="Floral decoration" class="flower flower-6"  />
            <img src="../assets/img/F3-verde.png" alt="Floral decoration" class="flower flower-7"   />
            <img src="../assets/img/F4-2.png" alt="Floral decoration" class="flower flower-8"   />
            <span class="number">{{ timeLeft.segundos }}</span>
            <span class="label">seg</span>
          </div>
        </div>
      </div>

      <!-- Sobre -->
      <div class="envelope">
        <img src="../assets/img/sobre-morado.png" alt="Wedding envelope" />
      </div>
      <div class="nombres">
        <img src="../assets/img/names.svg" alt="names" />
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;600&display=swap');

.wedding-app {
  background-image: url('../assets/img/fondo-abajo.png');
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center top;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0;
  position: relative;
  width: 100%;
}

.blur-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(10px);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.reveal-button {
  padding: 1.5rem 3rem;
  font-size: 1.8rem;
  background: linear-gradient(135deg, #6b4e8b, #9b7cb9);
  color: white;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  font-family: 'Cormorant Garamond', serif;
  letter-spacing: 3px;
  box-shadow: 0 4px 15px rgba(107, 78, 139, 0.3);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.reveal-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(107, 78, 139, 0.4);
}

.countdown-container {
  max-width: 700px;
  margin: 0 auto;
  text-align: center;
  padding: 2rem;
  opacity: 0;
  transition: opacity 1s ease;
}

.countdown-container.revealed {
  opacity: 1;
}

.music-controls {
  position: fixed;
  top: 2rem;
  right: 2rem;
  z-index: 100;
}

.music-button {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1.5rem;
  transition: transform 0.3s ease;
}

.music-button:hover {
  transform: scale(1.1);
}

.title {
  color: #4a4a4a;
  font-size: 1rem;
  margin-bottom: 2rem;
  line-height: 1.4;
  font-family: 'Cormorant Garamond', serif;
  text-transform: uppercase;
  letter-spacing: 1.2px;
  font-weight: 550;
  text-align: center;
}

.countdown-circles {
  margin-left: 18px;
  max-width: 400px;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 2rem;
  flex-wrap: wrap;
}

.countdown-item {
  position: relative;
}


.countdown-item .circle {
  width: 70px;
  height: 70px;
  border: 2px solid #d4af37;
  border-radius: 50%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  background: transparent;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  position: relative;
  transition: transform 0.3s ease;
}

.flower {
  position: absolute;
  width: 18px;
  height: auto;
  pointer-events: none;
}

.flower-1 { top: -10px; right: +13px;}
.flower-2 { top: -1px; right: 1px;}
.flower-3 { top: +5px; right: -5px; }
.flower-4 { top: +10px; right: -6px; }
.flower-5 { bottom: +14px; left: -15px; }
.flower-6 { bottom: +5px; left: -10px; }
.flower-7 { bottom: 1px; left: -1px; }
.flower-8 { bottom: -10px; left: +7px; }



.number {
  color: #4a4a4a;
  font-size: 1.1rem;
  line-height: 1.4;
  font-family: 'Cormorant Garamond', serif;
  text-transform: uppercase;
  letter-spacing: 2px;
  text-align: center;
}

.label {
  color: #4a4a4a;
  font-size: 1rem;
  margin-bottom: 0.1rem;
  line-height: 1.4;
  font-family: 'Cormorant Garamond', serif;
  letter-spacing: 2px;
  font-weight: 600;
  text-align: center;
}

.envelope {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 200px;
  height: auto;
  margin: 0 auto;
}

.envelope img {
  margin-top: 15px;
  width: 500%;
  max-width: 350px;
}


@keyframes float {
  0%, 100% {
    transform: translateY(0) rotate(0deg);
  }
  50% {
    transform: translateY(-10px) rotate(1deg);
  }
}

@keyframes flowerWave {
  0%, 100% {
    transform: rotate(0deg) translateY(0);
  }
  25% {
    transform: rotate(5deg) translateY(-2px);
  }
  75% {
    transform: rotate(-5deg) translateY(2px);
  }
}

.flower {

  animation: flowerWave 3s ease-in-out infinite;
}

.flower-1 { animation-delay: 0s; }
.flower-2 { animation-delay: 1s; }
.flower-3 { animation-delay: 2s; }
.flower-4 { animation-delay: 3s; }
.flower-5 { animation-delay: 4s; }
.flower-6 { animation-delay: 5s; }
.flower-7 { animation-delay: 6s; }
.flower-8 { animation-delay: 7s; }

@media (max-width: 768px) {
  .title {
    font-size: 1.8rem;
  }

  .countdown-item .circle {
    width: 120px;
    height: 120px;
  }

  .number {
    font-size: 2.5rem;
  }

  .envelope img {
    max-width: 300px;
  }

  html, body {
    margin: 0;
    padding: 0;
    overflow-x: hidden;
  }

}
</style>