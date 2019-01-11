
People / resources with great ideas

Best motion detector hacks ever

keatontaylor
https://community.home-assistant.io/t/motion-tracking-lights-lights-on-only-in-occupied-room/17744

* https://www.youtube.com/watch?v=kRRXve7HTpE 

* Serious YAML Foo

Great Visuals...
https://imgur.com/a/MvtUmwF

Frenck's list of awesome stuff 
https://github.com/frenck/awesome-home-assistant

OMG - her stuff is so good
https://github.com/isabellaalstrom


Mahasri Kalavala (git)
   @author         :   Mahasri Kalavala
   @date           :   11/19/2017
   @package        :   Timers and Lights/Switches
   @description    :   All lights and switches are now timer enabled
                       and the timer automatically extends when there is 
                       motion. When timer elapses, it turns off lights.

Customizing Icons:
https://community.home-assistant.io/t/mode-switch-input-select-button-redering/55934/8


customize:
  sensor.raspberry_pi:
    friendly_name: Raspberry Pi
    icon: mdi:raspberrypi
    templates:
      rgb_color: "if (state === 'Online') return [251, 210, 41]; else return [54, 95, 140];"