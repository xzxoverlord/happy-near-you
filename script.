const arr = []; // tree particles
const c = document.querySelector("#c");
const ctx = c.getContext("2d");
const cw = (c.width = 4000);
const ch = (c.height = 4000);
const T = Math.PI * 2;

const m = { x: cw / 2, y: 0 };
const xTo = gsap.quickTo(m, "x", { duration: 1.5, ease: "expo" });
const yTo = gsap.quickTo(m, "y", { duration: 1.5, ease: "expo" });


/* =========================
   SNOW CANVAS
========================= */
const arr2 = [];
const c2 = document.querySelector("#c2");
const ctx2 = c2.getContext("2d");
c2.width = c2.height = 4000;

/* =========================
   MOUSE PARALLAX
========================= */
c.addEventListener("pointermove", (e) => {
  const rect = c.getBoundingClientRect();
  const mouseX = e.x - rect.left;
  const mouseY = e.y - rect.top;
  const scaleX = c.width / rect.width;
  const scaleY = c.height / rect.height;
  xTo(mouseX * scaleX);
  yTo(mouseY * scaleY);
});

/* =========================
   TREE PARTICLES
========================= */
for (let i = 0; i < 999; i++) {
  arr.push({
    i,
    cx: cw / 2,
    cy: gsap.utils.mapRange(0, 999, 600, 3700, i),
    r: i < 900 ? gsap.utils.mapRange(0, 999, 3, 770, i) : 50,
    dot: 9,
    prog: 0.25,
    s: 1
  });

  const d = 99;
  arr[i].t = gsap
    .timeline({ repeat: -1 })
    .to(arr[i], { duration: d, prog: "+=1", ease: "slow(0.3, 0.4)" })
    .to(
      arr[i],
      {
        duration: d / 2,
        s: 0.15,
        repeat: 1,
        yoyo: true,
        ease: "power3.inOut"
      },
      0
    )
    .seek(Math.random() * d);

  arr2.push({
  x: cw * Math.random(),
  y: Math.random() * -ch,
  i: i,
  s: 1.5 + 6 * Math.random(),
  a: 0.15 + 0.6 * Math.random(),
  drift: -0.5 + Math.random(),
  type: Math.random() < 0.35 ? "flake" : "dot" // 35% hoa tuyết 6 cánh
});



  arr2[i].t = gsap
    .to(arr2[i], { ease: "none", y: ch, repeat: -1 })
    .seek(Math.random() * 99)
    .timeScale(arr2[i].s / 700);
}

/* =========================
   ⭐ STAR (TOP OF TREE)
========================= */
const star = {
  x: cw / 2,
  y: 560, // đỉnh cây
  r: 45,
  pulse: 0
};

gsap.to(star, {
  pulse: 1,
  duration: 1.6,
  ease: "sine.inOut",
  repeat: -1,
  yoyo: true
});

/* =========================
   RENDER LOOP
========================= */
gsap.ticker.add(render);

function render() {
  ctx.clearRect(0, 0, cw, ch);
  ctx2.clearRect(0, 0, cw, ch);

  arr.forEach(drawDot);

  //drawStar(star); // ⭐ VẼ SAO

  arr2.forEach(drawSnow);
}

/* =========================
   DRAW TREE DOT
========================= */
ctx.fillStyle = ctx2.fillStyle = "#fff";
ctx.strokeStyle = "rgba(255,255,255,0.05)";
ctx.globalCompositeOperation = "lighter";

function drawDot(c) {
  const angle = c.prog * T;
  const vs = 0.2;
  const x = Math.cos(angle) * c.r + c.cx;
  const y = Math.sin(angle) * c.r * vs + c.cy;
  const d = Math.hypot(x - m.x, y - m.y);
  const ms = gsap.utils.clamp(0.07, 1, d / cw);

  ctx.beginPath();
  ctx.arc(x, y, (c.dot * c.s) / 2 / ms, 0, T);
  ctx.fill();
  ctx.lineWidth = (c.dot * c.s * 2) / ms;
  ctx.stroke();
}

/* =========================
   DRAW STAR ⭐
========================= */
function drawStar(s) {
  const spikes = 5;
  const outer = s.r + s.pulse * 6;
  const inner = outer * 0.45;

  ctx.save();
  ctx.translate(s.x, s.y);

  // glow
  ctx.beginPath();
  ctx.arc(0, 0, outer * 2.5, 0, T);
  ctx.fillStyle = "rgba(255,255,255,0.15)";
  ctx.fill();

  // star shape
  let rot = Math.PI / 2 * 3;
  const step = Math.PI / spikes;

  ctx.beginPath();
  ctx.moveTo(0, -outer);
  for (let i = 0; i < spikes; i++) {
    ctx.lineTo(Math.cos(rot) * outer, Math.sin(rot) * outer);
    rot += step;
    ctx.lineTo(Math.cos(rot) * inner, Math.sin(rot) * inner);
    rot += step;
  }
  ctx.closePath();
  ctx.fillStyle = "#FFD700";
  ctx.fill();

  ctx.restore();
}

/* =========================
   DRAW SNOW
========================= */

function drawSnowflake(ctx, r) {
  ctx.beginPath();
  for (let i = 0; i < 6; i++) {
    ctx.save();
    ctx.rotate((Math.PI / 3) * i);

    ctx.moveTo(0, 0);
    ctx.lineTo(0, -r);

    // nhánh nhỏ
    ctx.moveTo(0, -r * 0.6);
    ctx.lineTo(-r * 0.2, -r * 0.4);
    ctx.moveTo(0, -r * 0.6);
    ctx.lineTo(r * 0.2, -r * 0.4);

    ctx.restore();
  }
  ctx.stroke();
}


function drawSnow(c) {
  const ys = gsap.utils.interpolate(1.4, 0.1, c.y / ch);

  ctx2.save();
  ctx2.translate(
    c.x + c.drift * c.y * 0.03,
    c.y
  );

  ctx2.rotate(30 * c.t.progress());
  ctx2.globalAlpha =
    c.a * ys * (0.6 + 0.4 * Math.sin(c.t.progress() * T * 2));

  if (c.type === "flake") {
    // ❄️ HOA TUYẾT 6 CÁNH
    ctx2.strokeStyle = "#fff";
    ctx2.lineWidth = 1.2 * ys;
    drawSnowflake(ctx2, c.s * 2.2 * ys);
  } else {
    // ❄️ TUYẾT TRÒN
    ctx2.beginPath();
    ctx2.arc(
      gsap.utils.interpolate(-80, 80, c.i / 2500),
      gsap.utils.interpolate(-40, 40, c.i / 2500),
      c.s * ys,
      0,
      T
    );
    ctx2.fill();
  }

  ctx2.restore();
}


/* =========================
   INTRO
========================= */
gsap.from(arr, {
  duration: 1,
  dot: 0,
  ease: "back.out(9)",
  stagger: -0.0009
});
gsap.from(m, {
  duration: 1.5,
  y: ch * 1.2,
  ease: "power2.inOut"
});
