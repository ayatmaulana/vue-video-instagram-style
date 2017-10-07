<template>
 
 <div :id="[id]">

    <div class="video-wrapper">
        <video  controls :poster="cover">
            <source :src="src"></source>
            Your browser does not support the <code>video</code> tag.
        </video>
        <div class="video-controls video-controls--show">
            <button data-media="play-pause">
              <i class="fa fa-play fa-4x" style="color: white;"></i>
            </button>
        </div>
    </div>

  </div>

</template>

<script>
import $ from 'jquery/dist/jquery.js'
export default {
  name: 'vue-video-instagram',
  props: [
    'src',
    'id',
    'cover',
    'mime'
  ],

  mounted()
  {
      console.log(111)
     'use strict';

          var InstaVideo = function (el) {
              
              this.$video    = $(el);
              this.$wrapper  = $(el).parent().addClass('paused');
              this.$controls = this.$wrapper.find('.video-controls');
              this.$controlIcon = this.$wrapper.find('.fa')

              // remove native controls
              this.$video.removeAttr('controls');
              
              // check if video should autoplay
              if(!!this.$video.attr('autoplay')) {
                  this.$wrapper.removeClass('paused').addClass('playing');
              }
              
              // check if video is muted
              if(this.$video.attr('muted') === 'true' || this.$video[0].volume === 0) {
                  this.$video[0].muted = true;
                  this.$wrapper.addClass('muted');
              }
              
              // attach event handlers
              this.attachEvents();
          };

          InstaVideo.prototype.attachEvents = function () {
            
              var self = this,
                  _t; // keep track of timeout for controls
              
              // attach handlers to data attributes
              this.$wrapper.on('click', '[data-media]', function () {

                  var data = $(this).data('media');

                  if(data === 'play-pause') {
                      self.playPause();
                  }
                  if(data === 'mute-unmute') {
                      self.muteUnmute();
                  }
              });
            
              this.$video.on('click', function () {
                  self.playPause();
              });
            
              this.$video.on('play', function () {
                  self.$wrapper.removeClass('paused').addClass('playing');
              });
              
              this.$video.on('pause', function () {
                  self.$wrapper.removeClass('playing').addClass('paused');
              });
            
              this.$video.on('volumechange', function () {
                  if($(this)[0].muted) {
                      self.$wrapper.addClass('muted');
                  }
                  else {
                      self.$wrapper.removeClass('muted');
                  }
              });
            
              this.$wrapper.on('mousemove', function () {
                    
                  // show controls
                  self.$controls.addClass('video-controls--show');

                  // clear original timeout
                  clearTimeout(_t);

                  // start a new one to hide controls after specified time
                  _t = setTimeout(function () {
                      self.$controls.removeClass('video-controls--show');
                  }, 2250);

              }).on('mouseleave', function () {
                  self.$controls.removeClass('video-controls--show');
              });
          };

          InstaVideo.prototype.playPause = function () {
              if (this.$video[0].paused) {
                  this.$controlIcon.addClass('fa-pause').removeClass('fa-play')
                  this.$video[0].play();
              } else { 
                  this.$controlIcon.addClass('fa-play').removeClass('fa-pause')
                  this.$video[0].pause();
              }
          };

          InstaVideo.prototype.muteUnmute = function () {
              if(this.$video[0].muted === false) {
                  this.$video[0].muted = true;
              } else {
                  this.$video[0].muted = false;
              }
          };

          $('video').each(function () {
              new InstaVideo(this);
          });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import './video.scss'
</style>
