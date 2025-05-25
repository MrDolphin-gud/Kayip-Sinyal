## Yapay Zeka Yardımı
https://chatgpt.com/share/682c716b-1b88-800e-9a71-431fdea19ef9

Bölüm tasarımında yardım alınmaya çalışılmıştır

const SCALE = 0.7;
  function scale(v) { return Math.round(v * SCALE); }

  const PLAYER_SIZE = scale(50);
  const GOAL_SIZE = scale(50);
  const OBSTACLE_SIZE = scale(50);

  // --- LEVEL DATA ---
  const levels = [
    // Level 1: Large rectangle, player right, goal center-left
    {
      room: { x: scale(250), y: scale(120), width: scale(600), height: scale(400), wall: scale(10) },
      player: { x: scale(700), y: scale(300) },
      goal: { x: scale(350), y: scale(300) },
      obstacles: []
    },
    // Level 2: L-shaped corridor
    {
      room: { x: scale(60), y: scale(60), width: scale(500), height: scale(500), wall: scale(10), lShape: true },
      player: { x: scale(460), y: scale(460) },
      goal: { x: scale(80), y: scale(80) },
      obstacles: []
    },
    // Level 3: Large square, two obstacles
    {
      room: { x: scale(100), y: scale(100), width: scale(530), height: scale(530), wall: scale(10) },
      player: { x: scale(570), y: scale(570) },
      goal: { x: scale(120), y: scale(120) },
      obstacles: [
        { x: scale(340), y: scale(220), size: scale(50) },
        { x: scale(220), y: scale(370), size: scale(50) }
      ]
    },
    // Level 4: Zigzag corridor
    {
      room: { x: scale(120), y: scale(220), width: scale(900), height: scale(250), wall: scale(10), zigzag: true },
      player: { x: scale(950), y: scale(250) },
      goal: { x: scale(140), y: scale(420) },
      obstacles: []
    }
  ]; levellerı düzeltmemiz lazım ilk level düz engelsiz playerın hiçbir yere gidemediği düz bir koridor olması gerek


ChatGPT:
Eğer birinci seviyenin amacı düz, engelsiz bir koridor olmasıysa ve oyuncunun hareket edebileceği alan sağdan sola uzanan bir koridor olacaksa, room, player ve goal konumlandırmalarını bu mantığa göre yeniden düzenlemek gerekir.

Aşağıda birinci level'i bu tarife uygun şekilde düzelttim:

const levels = [
  // Level 1: Straight, open corridor (horizontal)
  {
    room: { x: scale(100), y: scale(200), width: scale(800), height: scale(100), wall: scale(10) },
    player: { x: scale(850), y: scale(250) },
    goal: { x: scale(120), y: scale(250) },
    obstacles: []
  },
  // Level 2: L-shaped corridor
  {
    room: { x: scale(60), y: scale(60), width: scale(500), height: scale(500), wall: scale(10), lShape: true },
    player: { x: scale(460), y: scale(460) },
    goal: { x: scale(80), y: scale(80) },
    obstacles: []
  },
  // Level 3: Large square, two obstacles
  {
    room: { x: scale(100), y: scale(100), width: scale(530), height: scale(530), wall: scale(10) },
    player: { x: scale(570), y: scale(570) },
    goal: { x: scale(120), y: scale(120) },
    obstacles: [
      { x: scale(340), y: scale(220), size: scale(50) },
      { x: scale(220), y: scale(370), size: scale(50) }
    ]
  },
  // Level 4: Zigzag corridor
  {
    room: { x: scale(120), y: scale(220), width: scale(900), height: scale(250), wall: scale(10), zigzag: true },
    player: { x: scale(950), y: scale(250) },
    goal: { x: scale(140), y: scale(420) },
    obstacles: []
  }
];
Açıklama:
room: Uzunluğu 800, yüksekliği 100 olacak şekilde ayarlandı (yani yatay bir koridor).

player: En sağda yer aldı, x: 850 (800 + x:100 başlangıç + biraz boşluk).

goal: En solda yer aldı, x: 120 (x:100'e yakın).

obstacles: Boş (engelsiz).

İstersen ikinci leveli de sadeleştirip bir dikey koridora çevirebilirim. Devam edeyim mi?


Şunu dedin:
Ben hallederim tamam
ChatGPT:
Harika, kolay gelsin! Yardımcı olmamı istersen her zaman buradayım. Başka bir şey var mı?

Şunu dedin:
    if (room.lShape) {
 
      ctx.clearRect(room.x + scale(200), room.y, room.width - scale(200), scale(200));
      ctx.strokeRect(room.x + scale(200), room.y, room.width - scale(200), scale(200));
    } Sağ üstte bir kare kalıyor onu silmem lazım


ChatGPT:
Siz ctx.clearRect ve ctx.strokeRect ile L şeklini çizerken, sağ üst köşede kalan kareyi silememişsiniz çünkü sadece yatay kısmı temizliyorsunuz. Sağ üstte kalan o kare, muhtemelen scale(200) yüksekliğindeki dikey bir boşluğu tanımlamadığınız için kalıyor.

Şöyle düzeltebilirsiniz: Sağ üstte kalan kısmı da ayrıca clearRect ile temizleyin.

Aşağıdaki gibi yapın:
if (room.lShape) {
  // Yatay kısmı temizle (üstten sağa doğru giden koridorun dışını sil)
  ctx.clearRect(room.x + scale(200), room.y, room.width - scale(200), scale(200));
  ctx.strokeRect(room.x, room.y, room.width, room.height);

  // Sağ üstte kalan kareyi de temizle
  ctx.clearRect(room.x + scale(200), room.y, room.width - scale(200), room.height - scale(200));
}
Alternatif olarak, sadece L şeklinin içini çizmek istiyorsanız, doğrudan beginPath() ile L şeklini özel olarak çizebilirsiniz ama yukarıdaki çözüm, mevcut mantığınıza uygun ve hızlıdır.

Şunu dedin:
Bir ters L yapmaya çalışıyorum Ꞁ Bu şekilde
ChatGPT:
Harika, Ꞁ gibi bir ters L yapmak istiyorsanız yani:

Yukarıdan aşağı inen uzun bir dikey koridor (sol tarafta),

Ve onun alt kısmından sağa uzanan bir yatay koridor,

şöyle bir alan oluşmalı:
[ ]
[ ]
[ ]______
Bunu çizmek için önce tüm odayı çizip sonra sağ üst kısmı (olmaması gereken kısmı) clearRect ile temizlemeniz gerekir. Aşağıda bunu yapacak mantıklı kodu veriyorum:

✅ Ters L (Ꞁ) çizimi için örnek:
if (room.lShape) {
  // Odayı çerçeve olarak çiz
  ctx.strokeRect(room.x, room.y, room.width, room.height);

  // Sağ üst kısmı temizle → üstten yatay boşluk bırak
  ctx.clearRect(
    room.x + scale(200), // sağa kay
    room.y,              // en üstten başla
    room.width - scale(200), // kalan genişlik
    room.height - scale(200) // alt koridorun kalacağı kadar yüksekliği sil
  );
}
Açıklama:
scale(200) → dikey koridorun genişliği ve yatay koridorun yüksekliği gibi çalışıyor.

Sol dikey bacak ve alt yatay bacak kalıyor.

Sağ üstte kalan istenmeyen bölge temizleniyor.

Bu şekilde istediğiniz ters L (Ꞁ) şekli oluşur. Gerekirse bu şekli fillRect veya clip() ile de daha optimize çizebiliriz. Dilersen görsel de hazırlayabilirim — ister misiniz?
