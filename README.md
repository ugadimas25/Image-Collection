# Image-Collection
Image Collection menggunakan data landsat dan sentinel

```javascript
// 04 IMAGE COLLECTIONS

var l8sr = ee.ImageCollection("LANDSAT/LC08/C01/T1_SR"),
    geometry = /* color: #98ff00 */ ee.Geometry.Point([108.85, -7.68]);

// Pusat peta
Map.setCenter(108.85, -7.68, 12); // Segara Anakan, Cilacap
//Map.setCenter(108.7752969,-6.8054676,12); // Tawangsari, Cirebon

// Cari 'landsat 8 surface reflectance' dan impor data tier 1.
// Beri nama l8sr.
// Perhatikan bahwa Anda dapat menambahkannya ke peta:
Map.addLayer(l8sr, { bands: ['B4', 'B3', 'B2'], min: 0, max: 3000 }, 'l8sr');
// Ini secara implisit memanggil mosaic () untuk komposit bernilai baru.  
// Jelajahi koleksi dengan inspector.

// Gambar satu titik pada ROI Anda dengan alat geometri.

// Dapatkan citra dengan lokasi titik.
var spatialFiltered = l8sr.filterBounds(geometry);
// Ada berapa citra?
print('Number of images at the point', spatialFiltered.size());
// Jika ada kurang dari 5000, Anda dapat mencetaknya.
print('spatialFiltered', spatialFiltered);

// Dapatkan tanggal citra yang difilter pada saat itu.
var dateFiltered = spatialFiltered
    .filterDate('2018-01-01', '2018-12-31');
print('dateFiltered', dateFiltered);

// Urutkan berdasarkan properti yang menarik.
var sorted = dateFiltered.sort('CLOUD_COVER');

// Periksa citra pertama (berawan paling sedikit).
print('least cloudy image:', sorted.first());
