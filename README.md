# browserMine

const puppeteer = require('puppeteer');

(async () => {
const browser =  await puppeteer.launch();

console.log("monitoring start");
//while (true){
//   testMining();
//}

//async function testMining(browser) {
  console.log("browser start");
  
  const page = await browser.newPage();
  //await page.goto('https://google.com');
  //await page.screenshot({path: 'google.png'});
  await page.goto('https://mining.freebitco.in/mining.html?userid=10290802&auth=f6de70107812d689548f596e12278651c16715e42441349cac33b0eea2df0711');
  await page.screenshot({path: 'backup/montiorstart.png'});


  var counter = 0;
  var status = "START";
  while (true){
    timeString = new Date().toISOString().slice(0,19).replace(/[:T-]/g, "");
    console.log("monitoring: " + timeString );
    await page.waitFor(120000);
    await page.screenshot({path: 'backup/montior_running.png'});

    var result = await page.evaluate(() => {
      let hashrate = document.querySelector('#mining_user_hashrate').innerText;
      return {
        hashrate
      }
    });

    console.log(result.hashrate);

    if (result.hashrate == "0 H/s"){
      //var isStopButtonExist = await page.$('#stop_mining_button');
      //console.log(isStopButtonExist);
      //start button not exist
      if (counter == 3){
		  
        await page.goto('XXXX');
		await page.screenshot({path: 'backup/montior_reload'+timeString+'.png'});
		 
		 counter = 0;
		 await page.waitFor(5000);
		 status = "START";
		 continue;
      } else {
			counter = counter + 1;
			await page.waitFor(5000);			
	  }
		if (status == "START"){ 
			await page.click("#start_mining_button");
      //console.log(page);
      //await page.screenshot({path: 'backup/montior'+timeString+'2.png'});
			await page.waitFor(5000);
			const frames = await page.mainFrame().childFrames();
			console.log("show frame:" + frames.length);
			var iframeButtom = await frames[0].$("html body.light div#content div.actions button#accept.allow");
     // console.log(iframeButtom);
			iframeButtom.click();
      //await page.click("html body.light div#content div.actions button#accept.allow");
      //await page.waitFor(5000);
			console.log("mining start @" + new Date().toLocaleString());
			status = "STARTED";
			await page.waitFor(5000);
		}
    }
  }
//}



})();

process.on('SIGINT', function() {
  console.log("88");
  //browser.close();
  process.exit();
});




