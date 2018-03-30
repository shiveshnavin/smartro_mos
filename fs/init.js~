load('api_config.js');
load('api_gpio.js');
load('api_net.js');
load('api_sys.js');
load('api_timer.js');
load('api_http.js'); 
load('api_rpc.js');
load('api_adc.js');
 

let led = Cfg.get('pins.led');
let button = Cfg.get('pins.button');
let topic = '/devices/' + Cfg.get('device.id') + '/events';

print('LED GPIO:', led, 'button GPIO:', button);

let getInfo = function() {
  return JSON.stringify({
    total_ram: Sys.total_ram(),
    free_ram: Sys.free_ram()
  });
};



  Cfg.set( {wifi: {sta: {ssid: "JioFi2_00C3E7"}}} );
  Cfg.set( {wifi: {sta: {pass: "ytf47mnfjn"}}} );
  Cfg.set({wifi: {sta: {enable: true}}});
  print("WIFI CONFIGURED");

  Cfg.set( {wifi: {ap: {ssid: "SmartRo"}}} );              
  Cfg.set( {wifi: {ap: {pass: "password"}}} );
  Cfg.set({wifi: {ap: {enable: true}}});
  print("AP CONFIGURED");
  
  let adc=0;
  let rate=0;
/*************************MAIN*******************************************/

let blink=function(){
  
  GPIO.write(led,1); 
  Sys.usleep(300);
  GPIO.toggle(led);
}
  ADC.enable(adc);
  
  //print("START");
  let pin=5;
  GPIO.set_mode(pin, GPIO.MODE_INPUT);
  GPIO.set_int_handler(pin, GPIO.INT_EDGE_NEG, function(pin) {
   //print('Pin', pin, 'got interrupt');
   rate++;
    }, null);
  GPIO.enable_int(pin);
  
   
  GPIO.set_mode(led, GPIO.MODE_OUTPUT);
  Timer.set(1000 /* 1 sec */, Timer.REPEAT, function() {
    let value = GPIO.toggle(led);
    print('Interrupt Rate = ',rate,' RPM , TDS = ',ADC.read(adc));
    rate=0;
  }, null);
  
  
  RPC.addHandler('status',function(args){
    
    let val={
    interrupts_sec:rate,
    adc_val:ADC.read(adc), 
    total_ram: Sys.total_ram(),
    free_ram: Sys.free_ram()
    };
    blink();
    return val;
  })
  
  
  
  
  
  
  

/***************CONNECTION *******************************/

Net.setStatusEventHandler(function(ev, arg) {
  let evs = '???';
  init();
  if (ev === Net.STATUS_DISCONNECTED) {
    evs = 'DISCONNECTED';
  } else if (ev === Net.STATUS_CONNECTING) {
    evs = 'CONNECTING';
  } else if (ev === Net.STATUS_CONNECTED) {
    evs = 'CONNECTED';
    connected=1;
    init();
    print("IP : "+  Cfg.get("wifi.sta.ip"));
    
  } else if (ev === Net.STATUS_GOT_IP) {
    evs = 'GOT_IP';
  }
  print('== Net event:', ev, evs);

}, null);






