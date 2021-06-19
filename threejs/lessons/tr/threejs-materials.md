Title: Three.js Malzemeleri
Description: Three.js' de malzemeler
TOC: Malzemeler

Bu three.js ile alakalı makale serisinin bir parçasıdır. İlk makaleye buradan ulaşabilirsiniz [three.js fundamentals](threejs-fundamentals.html).
Eğer okumadıysanız ve three.js'e yeniyseniz buradan başlamayı düşünebilirsiniz.

Three.js birden çok malzeme tipine erişim sağlar.
Bu malzemeler objelerin sahnede nasıl göründüğünü tanımlar.
Hangi malzemei kullandığınız, ne yapmayı amaçladığınıza göre değişiklik gösterir.

Bir malzemein özelliklerini ayarlamak için 2 yöntem vardır. Birincisi daha önceden de gördüğümüz gibi oluşturulma anında.

```js
const material = new THREE.MeshPhongMaterial({
  color: 0xFF0000,    // Kırmızı (CSS renk komutları (color: red) da kullanılabilir)
  flatShading: true,
});
```

Öbür yöntem de oluşturulduktan sonra

```js
const material = new THREE.MeshPhongMaterial();
material.color.setHSL(0, 1, .5);  // kırmızı
material.flatShading = true;
```

`THREE.Color` tipinin özelliklerinin ayarlanması için birden fazla yol olduğunu unutmayın.

```js
material.color.set(0x00FFFF);    // CSS'in #RRGGBB sitiliyle aynı
material.color.set(cssString);   // Herhangi bir CSS rengi, eg 'purple', '#F32',
                                 // 'rgb(255, 127, 64)',
                                 // 'hsl(180, 50%, 25%)'
material.color.set(someColor)    // Başka bir THREE.Color
material.color.setHSL(h, s, l)   // h, s, ve l, 0 ya da 1
material.color.setRGB(r, g, b)   // r, g, ve b, 0 ya da 1
```

Ve oluşturma zamanında bir onaltılık sayı veya bir CSS dizesi iletebilirsiniz.

```js
const m1 = new THREE.MeshBasicMaterial({color: 0xFF0000});         // kırmızı
const m2 = new THREE.MeshBasicMaterial({color: 'red'});            // kırmızı
const m3 = new THREE.MeshBasicMaterial({color: '#F00'});           // kırmızı
const m4 = new THREE.MeshBasicMaterial({color: 'rgb(255,0,0)'});   // kırmızı
const m5 = new THREE.MeshBasicMaterial({color: 'hsl(0,100%,50%)'); // kırmızı
```

Şimdi three.js'in malzeme setleri üzerinden geçelim.

`MeshBasicMaterial` ışıklardan etkilenmez.
`MeshLambertMaterial` ışığı sadece köşelerde hesaplar. `MeshPhongMaterial` ise ışığı her pixelde hesaplar. `MeshPhongMaterial` da aynasal vurgulamaları destekler.

<div class="spread">
  <div>
    <div data-diagram="MeshBasicMaterial" ></div>
    <div class="code">Basic</div>
  </div>
  <div>
    <div data-diagram="MeshLambertMaterial" ></div>
    <div class="code">Lambert</div>
  </div>
  <div>
    <div data-diagram="MeshPhongMaterial" ></div>
    <div class="code">Phong</div>
  </div>
</div>
<div class="spread">
  <div>
    <div data-diagram="MeshBasicMaterialLowPoly" ></div>
  </div>
  <div>
    <div data-diagram="MeshLambertMaterialLowPoly" ></div>
  </div>
  <div>
    <div data-diagram="MeshPhongMaterialLowPoly" ></div>
  </div>
</div>
<div class="threejs_center code">aynı malzemelere sahip düşük poligon modeller</div>

`MeshPhongMaterial` da ki `shininess` ayarı belirli bir aynasal vurgunun *parlaklığını* ayarlar. Varsayılan değeri 30 dur.

<div class="spread">
  <div>
    <div data-diagram="MeshPhongMaterialShininess0" ></div>
    <div class="code">shininess: 0</div>
  </div>
  <div>
    <div data-diagram="MeshPhongMaterialShininess30" ></div>
    <div class="code">shininess: 30</div>
  </div>
  <div>
    <div data-diagram="MeshPhongMaterialShininess150" ></div>
    <div class="code">shininess: 150</div>
  </div>
</div>

Unutmayın ki `emissive` özelliğini rengini `MeshLambertMaterial` ya da `MeshPhongMaterial` dab ayarlayarak ve `color`'ı siyaha ayarlamak (phong için `shininess` 0)
`MeshBasicMaterial` gibi gözükür.

<div class="spread">
  <div>
    <div data-diagram="MeshBasicMaterialCompare" ></div>
    <div class="code">
      <div>Basic</div>
      <div>color: 'purple'</div>
    </div>
  </div>
  <div>
    <div data-diagram="MeshLambertMaterialCompare" ></div>
    <div class="code">
      <div>Lambert</div>
      <div>color: 'black'</div>
      <div>emissive: 'purple'</div>
    </div>
  </div>
  <div>
    <div data-diagram="MeshPhongMaterialCompare" ></div>
    <div class="code">
      <div>Phong</div>
      <div>color: 'black'</div>
      <div>emissive: 'purple'</div>
      <div>shininess: 0</div>
    </div>
  </div>
</div>

Şimdi aklınızda bir soru kalmış olabilir. `MeshPhongMaterial`, `MeshBasicMaterial` ve `MeshLambertMaterial` ile aynı şeyleri yapabildiği halde
neden bu üçüne de ihtiyacımız var ? Bunun nedeni, daha sofistike malzemeler
çizmek için daha fazla GPU gücü alır. Cep telefonu gibi daha yavaş bir GPU'da
sahnenizi çizmek için gereken GPU gücünü daha az karmaşık malzemelerden birini kullanarak azaltmak isteyebilirsiniz.
Bununla birlikte eğer ekstra özelliklere ihtiyacınız yoksa en basit malzemeyi kullanabilirsiniz.
Eğer ışıklandırmaya ya da aynasal vurguya ihtiyacınız yoksa `MeshBasicMaterial` kullanabilirsiniz.

`MeshToonMaterial` bir büyük fark dışında `MeshPhongMaterial` a benzer. Bu farkı da `MeshToonMaterial`
düzgün bir şekilde gölgelendirmek yerine, nasıl gölgeleneceğine karar vermek için bir gradyan haritası kullanır
(X'e 1 doku). Varsayılan, bir gradyan haritası değerleri ilk %70 i için %70 parlaklık ve sonrası için %100 dür.
Ancak kendi gradyan haritanızı ayarlayabilirsiniz. Bu da çizgi film gibi görünen 2 adet ton oluşturuyor.

<div class="spread">
  <div data-diagram="MeshToonMaterial"></div>
</div>

Sırada 2 *fiziksel tabanlı oluşturma* malzemesi var. 

The materials above use simple math to make materials that look 3D but they
aren't what actually happens in real world. The 2 PBR materials use much more
complex math to come close to what actually happens in the real world.

The first one is `MeshStandardMaterial`. The biggest difference between
`MeshPhongMaterial` and `MeshStandardMaterial` is it uses different parameters.
`MeshPhongMaterial` had a `shininess` setting. `MeshStandardMaterial` has 2
settings `roughness` and `metalness`.

At a basic level [`roughness`](MeshStandardMaterial.roughness) is the opposite
of `shininess`. Something that has a high roughness, like a baseball doesn't
have hard reflections whereas something that's not rough, like a billiard ball,
is very shiny. Roughness goes from 0 to 1.

The other setting, [`metalness`](MeshStandardMaterial.metalness), says
how metal the material is. Metals behave differently than non-metals. 0
for non-metal and 1 for metal.

Here's a quick sample of `MeshStandardMaterial` with `roughness` from 0 to 1
across and `metalness` from 0 to 1 down.

<div data-diagram="MeshStandardMaterial" style="min-height: 400px"></div>

The `MeshPhysicalMaterial` is same as the `MeshStandardMaterial` but it
adds a `clearcoat` parameter that goes from 0 to 1 for how much to
apply a clearcoat gloss layer and a `clearCoatRoughness` parameter
that specifies how rough the gloss layer is.

Here's the same grid of `roughness` by `metalness` as above but with
`clearcoat` and `clearCoatRoughness` settings.

<div data-diagram="MeshPhysicalMaterial" style="min-height: 400px"></div>

The various standard materials progress from fastest to slowest
`MeshBasicMaterial` ➡ `MeshLambertMaterial` ➡ `MeshPhongMaterial` ➡
`MeshStandardMaterial` ➡ `MeshPhysicalMaterial`. The slower materials
can make more realistic looking scenes but you might need to design
your code to use the faster materials on low powered or mobile machines.

There are 3 materials that have special uses. `ShadowMaterial`
is used to get the data created from shadows. We haven't
covered shadows yet. When we do we'll use this material
to take a peek at what's happening behind the scenes.

The `MeshDepthMaterial` renders the depth of each pixel where
pixels at negative [`near`](PerspectiveCamera.near) of the camera are 0 and negative [`far`](PerspectiveCamera.far) are 1. Certain special effects can use this data which we'll
get into at another time.

<div class="spread">
  <div>
    <div data-diagram="MeshDepthMaterial"></div>
  </div>
</div>

The `MeshNormalMaterial` will show you the *normals* of geometry.
*Normals* are the direction a particular triangle or pixel faces.
`MeshNormalMaterial` draws the view space normals (the normals relative to the camera).
<span style="background: red;" class="color">x is red</span>,
<span style="background: green;" class="dark-color">y is green</span>, and
<span style="background: blue;" class="dark-color">z is blue</span> so things facing
to the right will be <span style="background: #FF7F7F;" class="color">pink</span>,
to the left will be <span style="background: #007F7F;" class="dark-color">aqua</span>,
up will be <span style="background: #7FFF7F;" class="color">light green</span>,
down will be <span style="background: #7F007F;" class="dark-color">purple</span>,
and toward the screen will be <span style="background: #7F7FFF;" class="color">lavender</span>.

<div class="spread">
  <div>
    <div data-diagram="MeshNormalMaterial"></div>
  </div>
</div>

`ShaderMaterial` is for making custom materials using the three.js shader
system. `RawShaderMaterial` is for making entirely custom shaders with
no help from three.js. Both of these topics are large and will be
covered later.

Most materials share a bunch of settings all defined by `Material`.
[See the docs](Material)
for all of them but let's go over two of the most commonly used
properties.

[`flatShading`](Material.flatShading):
whether or not the object looks faceted or smooth. default = `false`.

<div class="spread">
  <div>
    <div data-diagram="smoothShading"></div>
    <div class="code">flatShading: false</div>
  </div>
  <div>
    <div data-diagram="flatShading"></div>
    <div class="code">flatShading: true</div>
  </div>
</div>

[`side`](Material.side): which sides of triangles to show. The default is `THREE.FrontSide`.
Other options are `THREE.BackSide` and `THREE.DoubleSide` (both sides).
Most 3D objects drawn in three are probably opaque solids so the back sides
(the sides facing inside the solid) do not need to be drawn. The most common
reason to set `side` is for planes or other non-solid objects where it is
common to see the back sides of triangles.

Here are 6 planes drawn with `THREE.FrontSide` and `THREE.DoubleSide`.

<div class="spread">
  <div>
    <div data-diagram="sideDefault" style="height: 250px;"></div>
    <div class="code">side: THREE.FrontSide</div>
  </div>
  <div>
    <div data-diagram="sideDouble" style="height: 250px;"></div>
    <div class="code">side: THREE.DoubleSide</div>
  </div>
</div>

There's really a lot to consider with materials and we actually still
have a bunch more to go. In particular we've mostly ignored textures
which open up a whole slew of options. Before we cover textures though
we need to take a break and cover
[setting up your development environment](threejs-setup.html)

<div class="threejs_bottombar">
<h3>material.needsUpdate</h3>
<p>
This topic rarely affects most three.js apps but just as an FYI...
Three.js applies material settings when a material is used where "used"
means "something is rendered that uses the material". Some material settings are
only applied once as changing them requires lots of work by three.js.
In those cases you need to set <code>material.needsUpdate = true</code> to tell
three.js to apply your material changes. The most common settings
that require you to set <code>needsUpdate</code> if you change the settings after
using the material are:
</p>
<ul>
  <li><code>flatShading</code></li>
  <li>adding or removing a texture
    <p>
    Changing a texture is ok, but if want to switch from using no texture
    to using a texture or from using a texture to using no texture
    then you need to set <code>needsUpdate = true</code>.
    </p>
    <p>In the case of going from texture to no-texture it is often
    just better to use a 1x1 pixel white texture.</p>
  </li>
</ul>
<p>As mentioned above most apps never run into these issues. Most apps
do not switch between flat shaded and non flat shaded. Most apps also
either use textures or a solid color for a given material, they rarely
switch from using one to using the other.
</p>
</div>

<canvas id="c"></canvas>
<script type="module" src="resources/threejs-materials.js"></script>

