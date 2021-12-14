<template>
  <div class="w-2/3 h-96 my-10 mx-auto relative">
    <video
      id="video-player"
      controls
      autoplay
      muted
      class="cld-video-player cld-video-player-skin-dark w-full h-full"
    >
    </video>
    <a 
      v-if="percentage > 20 && percentage < 70"
      href="https://www.pexels.com/video/aerial-view-on-city-during-dawn-9722139/" 
      target="_blank"
      class="inline-flex items-center px-4 py-2 opacity-50 hover:opacity-70 border border-gray-300 shadow-sm text-sm font-medium rounded-md text-white bg-transparent hover:text-black hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 absolute top-10 right-10"
    >
      View on Pexels
    </a>
    <img 
      v-if="paused || ended"
      src="https://res.cloudinary.com/hackit-africa/image/upload/v1639495990/nuxtjs-in-video-cta/free-video-856479.jpg" 
      class="absolute w-96 top-16 left-72"
    />
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data(){
    return {
      cld:null,
      player:null,
      video: "nuxtjs-in-video-cta/pexels-kelly-lacy-9722139.mp4",
      percentage:null,
      paused:null,
      ended:null
    }
  },
  mounted(){
    this.cld = cloudinary.Cloudinary.new({ 
      cloud_name: process.env.NUXT_ENV_CLOUDINARY_CLOUD_NAME, 
      secure: true
    });

    this.player = this.cld.videoPlayer(
      'video-player',
      {
        playedEventTimes: [10,20,30,40,50,60,70,80,90],
        analytics: {
          events: [ 
            'play',   
            'pause',
            'percentsplayed',
            'start',
            'ended',
          ]
        }
      }
    );
    
    this.player.source(this.video);

    this.player.on('percentsplayed', (event) => {
      this.percentage = event.eventData.percent;
    });

    this.player.on('play', () => {
      this.paused = false;
      this.ended = false;
    });

    this.player.on('pause', () => {
      this.paused = true;
    });

    this.player.on('start', () => {
      this.paused = false;
      this.ended = false;
    });

    this.player.on('ended', () => {
      this.ended = true;
    });

  }
};
</script>
