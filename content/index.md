---
banner: "https://i.pinimg.com/originals/2d/bc/f5/2dbcf578101b43249c6aa3f009d4b97f.jpg"
banner_y: 0.368
banner_lock: true
---
He saved millions, but he cannot save himself. ◢◤ Forever

[[Sunshine Moonlight and Moondeck Buddy]]

---
>[!multi-column]
>> [!blank-container|center]
>> # Weather
>> ```dataviewjs
>> const {Weather} = customJS;
>> let element = this.container.createEl('div', {cls: ["tailwind"]});
>> await Weather.getWeather(element, 91754, String.raw`C:\Users\4542e\Desktop\obsidian_personal\_data_\script\javascript\cache\weather_cache.json`);
>> ```
>
>> [!blank-container]
>> # Inspirational Quote
>> ```dataviewjs
>> const {ZenQuotes} = customJS;
>> let element = this.container.createEl('div', {cls: ["tailwind"]});
>> await ZenQuotes.getQuote(element, String.raw`C:\Users\4542e\Desktop\obsidian_personal\_data_\script\javascript\cache\quote_cache.json`); 
>> ```
---
# Recently Changed
```dataviewjs
function converteTime(time){
// Convert from ms to minutes
let convertedTime = ""
time = time/60000; // time in minutes

if(time < 60){
convertedTime = `${Math.ceil(time)} minutes ago`;
} else if (time < 1440){
convertedTime = `${Math.ceil(time/60)} hours ago`
} else {
convertedTime = `${Math.ceil(time/1440)} days ago`
}	
return convertedTime
}

for (let group of dv.pages('!"_data_"').sort(k => k.file.mtime, 'desc').limit(5).groupBy(p => p.page)) {
dv.table(["Name", "Date Modified", ""], 
group.rows
.sort(k => k.file.mtime, 'desc')
.map(k => [
k.file.link, 
converteTime(Date.now()-k.file.mtime)
]))}
```

---
# Hacker News (Top stories)
```dataviewjs
const {News} = customJS;
let element = this.container.createEl('div', {cls: ["tailwind"]});
await News.listNews(element, 15, String.raw`C:\Users\4542e\Desktop\obsidian_personal\_data_\script\javascript\cache\news_cache.json`);
```
