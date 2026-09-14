[planner_sw.js](https://github.com/user-attachments/files/32214679/planner_sw.js)
// 7 Habits Planner — Service Worker
const CACHE = 'planner-v1';
const FILES = [
  './',
  './index.html',
  './7habits_planner_v5.html'
];

// 설치 — 핵심 파일 캐시
self.addEventListener('install', function(e){
  e.waitUntil(
    caches.open(CACHE).then(function(cache){
      return cache.addAll(FILES);
    })
  );
  self.skipWaiting();
});

// 활성화 — 이전 캐시 삭제
self.addEventListener('activate', function(e){
  e.waitUntil(
    caches.keys().then(function(keys){
      return Promise.all(
        keys.filter(function(k){ return k!==CACHE; })
            .map(function(k){ return caches.delete(k); })
      );
    })
  );
  self.clients.claim();
});

// 요청 — Network First (항상 최신 버전 우선)
self.addEventListener('fetch', function(e){
  e.respondWith(
    fetch(e.request)
      .then(function(res){
        var clone=res.clone();
        caches.open(CACHE).then(function(cache){
          cache.put(e.request, clone);
        });
        return res;
      })
      .catch(function(){
        return caches.match(e.request);
      })
  );
});
