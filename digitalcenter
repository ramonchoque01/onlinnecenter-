import React, { useMemo, useState } from "react";
import { motion, AnimatePresence } from "framer-motion";
import { ShoppingCart, Search, Filter, Plus, Minus, X, Store, Trash2 } from "lucide-react";
import { Button } from "@/components/ui/button";
import { Card, CardContent, CardFooter, CardHeader } from "@/components/ui/card";
import { Badge } from "@/components/ui/badge";
import { Input } from "@/components/ui/input";
import { Sheet, SheetContent, SheetTrigger } from "@/components/ui/sheet";

// ---------- Utilidades ----------
const formatBs = (num) => new Intl.NumberFormat("es-BO", { style: "currency", currency: "BOB" }).format(num);
const slug = (s) => s.toLowerCase().normalize("NFD").replace(/\p{Diacritic}/gu, "").replace(/[^a-z0-9]+/g, " ").trim();

// ---------- Datos de ejemplo (modifica con tus productos HORMART) ----------
const CATEGORIES = ["Todos", "Hogar", "Electro", "Cocina", "Limpieza", "Oficina"];
const PRODUCTS = [
  {
    id: 1,
    name: "Licuadora Hormart Pro 900W",
    category: "Cocina",
    price: 429,
    stock: 18,
    image: "https://images.unsplash.com/photo-1586201375761-83865001e31b?q=80&w=1200&auto=format&fit=crop",
    tags: ["potente", "acero"],
  },
  {
    id: 2,
    name: "Set de Sartenes Antiadherentes (3 pzs)",
    category: "Cocina",
    price: 299,
    stock: 24,
    image: "https://images.unsplash.com/photo-1505576383114-b7ebcd0b3416?q=80&w=1200&auto=format&fit=crop",
    tags: ["antiadherente", "familia"],
  },
  {
    id: 3,
    name: "Aspiradora Hormart TurboClean",
    category: "Limpieza",
    price: 649,
    stock: 9,
    image: "https://images.unsplash.com/photo-1581578731548-c64695cc6952?q=80&w=1200&auto=format&fit=crop",
    tags: ["turbo", "filtro hepa"],
  },
  {
    id: 4,
    name: "Hervidor Eléctrico 1.7L",
    category: "Electro",
    price: 149,
    stock: 35,
    image: "https://images.unsplash.com/photo-1604908554007-1f66e0a5e7b8?q=80&w=1200&auto=format&fit=crop",
    tags: ["acero", "rápido"],
  },
  {
    id: 5,
    name: "Escritorio Compacto Hormart",
    category: "Oficina",
    price: 559,
    stock: 7,
    image: "https://images.unsplash.com/photo-1587300003388-59208cc962cb?q=80&w=1200&auto=format&fit=crop",
    tags: ["madera", "minimal"],
  },
  {
    id: 6,
    name: "Kit Limpieza Multiusos (6 pzs)",
    category: "Limpieza",
    price: 119,
    stock: 40,
    image: "https://images.unsplash.com/photo-1581579188871-45ea61f2a0c8?q=80&w=1200&auto=format&fit=crop",
    tags: ["hogar", "eco"],
  },
];

// ---------- Componentes ----------
function ProductCard({ product, onAdd }) {
  return (
    <motion.div layout initial={{ opacity: 0, y: 12 }} animate={{ opacity: 1, y: 0 }} exit={{ opacity: 0 }}>
      <Card className="overflow-hidden rounded-2xl shadow-sm hover:shadow-md transition">
        <div className="aspect-[4/3] overflow-hidden bg-gray-100">
          <img src={product.image} alt={product.name} className="h-full w-full object-cover" />
        </div>
        <CardHeader className="pb-2">
          <div className="flex items-start justify-between gap-3">
            <h3 className="text-base font-semibold leading-tight line-clamp-2">{product.name}</h3>
            <Badge variant="secondary" className="shrink-0">{product.category}</Badge>
          </div>
        </CardHeader>
        <CardContent className="pt-0 text-sm text-muted-foreground">
          <div className="flex flex-wrap gap-2">
            {product.tags?.map((t) => (
              <Badge key={t} variant="outline" className="rounded-full">#{t}</Badge>
            ))}
          </div>
          <div className="mt-3 flex items-center justify-between">
            <span className="text-lg font-bold">{formatBs(product.price)}</span>
            <span className={`text-xs ${product.stock > 10 ? "text-emerald-600" : product.stock > 0 ? "text-amber-600" : "text-rose-600"}`}>
              {product.stock > 10 ? "En stock" : product.stock > 0 ? `Pocas unidades (${product.stock})` : "Agotado"}
            </span>
          </div>
        </CardContent>
        <CardFooter className="pt-0">
          <Button className="w-full" onClick={() => onAdd(product)} disabled={product.stock === 0}>
            <Plus className="mr-2 h-4 w-4" /> Agregar al carrito
          </Button>
        </CardFooter>
      </Card>
    </motion.div>
  );
}

function Cart({ items, onAdd, onRemove, onClear }) {
  const total = items.reduce((s, it) => s + it.price * it.qty, 0);
  return (
    <div className="flex h-full flex-col">
      <div className="flex items-center justify-between border-b p-4">
        <h3 className="text-lg font-semibold">Tu carrito</h3>
        <Button variant="ghost" size="icon" onClick={onClear} disabled={!items.length}>
          <Trash2 className="h-5 w-5" />
        </Button>
      </div>
      <div className="flex-1 space-y-3 overflow-y-auto p-4">
        <AnimatePresence>
          {items.length === 0 && (
            <motion.p initial={{ opacity: 0 }} animate={{ opacity: 1 }} className="text-sm text-muted-foreground">
              Aún no tienes productos. ¡Explora el catálogo!
            </motion.p>
          )}
          {items.map((it) => (
            <motion.div key={it.id} layout initial={{ opacity: 0, y: 6 }} animate={{ opacity: 1, y: 0 }} exit={{ opacity: 0 }} className="flex gap-3 rounded-xl border p-3">
              <img src={it.image} alt={it.name} className="h-16 w-16 rounded-lg object-cover" />
              <div className="min-w-0 flex-1">
                <p className="truncate text-sm font-medium">{it.name}</p>
                <p className="text-xs text-muted-foreground">{formatBs(it.price)} c/u</p>
                <div className="mt-2 flex items-center gap-2">
                  <Button size="icon" variant="outline" onClick={() => onRemove(it)}>
                    <Minus className="h-4 w-4" />
                  </Button>
                  <span className="w-8 text-center text-sm">{it.qty}</span>
                  <Button size="icon" onClick={() => onAdd(it)}>
                    <Plus className="h-4 w-4" />
                  </Button>
                </div>
              </div>
              <div className="ml-auto text-right">
                <p className="text-sm font-semibold">{formatBs(it.price * it.qty)}</p>
              </div>
            </motion.div>
          ))}
        </AnimatePresence>
      </div>
      <div className="border-t p-4">
        <div className="mb-3 flex items-center justify-between text-sm">
          <span>Subtotal</span>
          <span className="font-semibold">{formatBs(total)}</span>
        </div>
        <Button className="w-full" disabled={!items.length}>Proceder al pago</Button>
        <p className="mt-2 text-center text-xs text-muted-foreground">* Ejemplo de demo. Integra tu pasarela (QR, tarjeta, contra‑entrega).</p>
      </div>
    </div>
  );
}

// ---------- Página principal ----------
export default function HormartStorefront() {
  const [query, setQuery] = useState("");
  const [category, setCategory] = useState("Todos");
  const [maxPrice, setMaxPrice] = useState(1000);
  const [sort, setSort] = useState("relevance");
  const [cartOpen, setCartOpen] = useState(false);
  const [cart, setCart] = useState([]);

  const filtered = useMemo(() => {
    let list = PRODUCTS.filter((p) => (category === "Todos" ? true : p.category === category));
    if (query.trim()) {
      const q = slug(query);
      list = list.filter((p) => slug(p.name + " " + p.category + " " + (p.tags || []).join(" ")).includes(q));
    }
    list = list.filter((p) => p.price <= maxPrice);
    switch (sort) {
      case "price_asc":
        list = [...list].sort((a, b) => a.price - b.price);
        break;
      case "price_desc":
        list = [...list].sort((a, b) => b.price - a.price);
        break;
      case "alpha":
        list = [...list].sort((a, b) => a.name.localeCompare(b.name));
        break;
      default:
        break;
    }
    return list;
  }, [query, category, maxPrice, sort]);

  const addToCart = (product) => {
    setCart((prev) => {
      const idx = prev.findIndex((p) => p.id === product.id);
      if (idx >= 0) {
        const next = [...prev];
        next[idx] = { ...next[idx], qty: next[idx].qty + 1 };
        return next;
      }
      return [...prev, { ...product, qty: 1 }];
    });
    setCartOpen(true);
  };

  const removeFromCart = (product) => {
    setCart((prev) => {
      const idx = prev.findIndex((p) => p.id === product.id);
      if (idx === -1) return prev;
      const next = [...prev];
      const newQty = next[idx].qty - 1;
      if (newQty <= 0) next.splice(idx, 1);
      else next[idx] = { ...next[idx], qty: newQty };
      return next;
    });
  };

  const clearCart = () => setCart([]);

  return (
    <div className="min-h-screen bg-gradient-to-b from-white to-slate-50">
      {/* Header */}
      <header className="sticky top-0 z-30 bg-white/80 backdrop-blur border-b">
        <div className="mx-auto flex max-w-7xl items-center justify-between gap-4 px-4 py-3">
          <div className="flex items-center gap-2">
            <Store className="h-6 w-6" />
            <span className="text-xl font-bold tracking-tight">HORMART</span>
            <span className="hidden pl-2 text-xs text-muted-foreground md:inline">Calidad para tu hogar</span>
          </div>
          <div className="flex items-center gap-2">
            <div className="relative hidden md:block">
              <Search className="pointer-events-none absolute left-3 top-1/2 h-4 w-4 -translate-y-1/2 text-gray-400" />
              <Input value={query} onChange={(e) => setQuery(e.target.value)} placeholder="Buscar productos…" className="w-[320px] rounded-full pl-9" />
            </div>
            <Sheet open={cartOpen} onOpenChange={setCartOpen}>
              <SheetTrigger asChild>
                <Button variant="outline" className="rounded-full">
                  <ShoppingCart className="mr-2 h-4 w-4" /> Carrito ({cart.reduce((s, i) => s + i.qty, 0)})
                </Button>
              </SheetTrigger>
              <SheetContent side="right" className="w-full sm:w-[420px] p-0">
                <Cart items={cart} onAdd={addToCart} onRemove={removeFromCart} onClear={clearCart} />
              </SheetContent>
            </Sheet>
          </div>
        </div>
      </header>

      {/* Hero */}
      <section className="mx-auto max-w-7xl px-4">
        <div className="mt-8 grid gap-6 rounded-3xl bg-slate-900 p-8 text-white md:grid-cols-3">
          <div className="md:col-span-2">
            <h1 className="text-3xl font-extrabold tracking-tight md:text-4xl">Todo para tu hogar con <span className="text-amber-300">HORMART</span></h1>
            <p className="mt-3 text-white/80">Catálogo curado, entregas rápidas y garantía real. Compra en línea y recibe en tu casa.</p>
            <div className="mt-5 flex flex-wrap gap-3">
              <Button onClick={() => document.getElementById("catalogo")?.scrollIntoView({ behavior: "smooth" })}>Ver catálogo</Button>
              <Button variant="secondary" onClick={() => setCartOpen(true)}>Revisar carrito</Button>
            </div>
          </div>
          <div className="relative hidden overflow-hidden rounded-2xl md:block">
            <img src="https://images.unsplash.com/photo-1519710164239-da123dc03ef4?q=80&w=1200&auto=format&fit=crop" alt="HORMART hero" className="h-full w-full object-cover opacity-90" />
            <div className="absolute inset-0 bg-gradient-to-t from-slate-900/60 to-transparent" />
          </div>
        </div>
      </section>

      {/* Filtros */}
      <section className="mx-auto max-w-7xl px-4">
        <div className="mt-8 rounded-3xl border bg-white p-4 shadow-sm">
          <div className="grid gap-4 md:grid-cols-4">
            <div className="md:col-span-2 flex items-center gap-2 md:hidden">
              <Search className="h-4 w-4 text-gray-500" />
              <Input value={query} onChange={(e) => setQuery(e.target.value)} placeholder="Buscar productos…" className="w-full" />
            </div>
            <div className="flex items-center gap-2">
              <Filter className="h-4 w-4 text-gray-500" />
              <select value={category} onChange={(e) => setCategory(e.target.value)} className="w-full rounded-xl border px-3 py-2 text-sm">
                {CATEGORIES.map((c) => (
                  <option key={c} value={c}>{c}</option>
                ))}
              </select>
            </div>
            <div className="flex items-center gap-3">
              <span className="whitespace-nowrap text-sm text-gray-600">Precio máx:</span>
              <input type="range" min={50} max={1000} step={10} value={maxPrice} onChange={(e) => setMaxPrice(Number(e.target.value))} className="w-full" />
              <span className="w-20 text-right text-sm font-medium">{formatBs(maxPrice)}</span>
            </div>
            <div className="flex items-center gap-2">
              <span className="whitespace-nowrap text-sm text-gray-600">Ordenar:</span>
              <select value={sort} onChange={(e) => setSort(e.target.value)} className="w-full rounded-xl border px-3 py-2 text-sm">
                <option value="relevance">Relevancia</option>
                <option value="price_asc">Precio: menor a mayor</option>
                <option value="price_desc">Precio: mayor a menor</option>
                <option value="alpha">Alfabético</option>
              </select>
            </div>
          </div>
        </div>
      </section>

      {/* Catálogo */}
      <section id="catalogo" className="mx-auto max-w-7xl px-4">
        <div className="mb-4 mt-8 flex items-baseline justify-between">
          <h2 className="text-xl font-bold tracking-tight">Catálogo</h2>
          <p className="text-sm text-muted-foreground">{filtered.length} producto(s) encontrado(s)</p>
        </div>
        <motion.div layout className="grid gap-5 sm:grid-cols-2 lg:grid-cols-3">
          <AnimatePresence>
            {filtered.map((p) => (
              <ProductCard key={p.id} product={p} onAdd={addToCart} />
            ))}
          </AnimatePresence>
        </motion.div>
      </section>

      {/* Footer */}
      <footer className="mt-16 border-t">
        <div className="mx-auto max-w-7xl px-4 py-10 text-sm text-muted-foreground">
          <div className="flex flex-wrap items-center justify-between gap-3">
            <p>© {new Date().getFullYear()} HORMART. Todos los derechos reservados.</p>
            <div className="flex flex-wrap items-center gap-3">
              <a className="hover:underline" href="#">Políticas</a>
              <a className="hover:underline" href="#">Términos</a>
              <a className="hover:underline" href="#">Soporte</a>
            </div>
          </div>
          <p className="mt-2 text-xs">Demo UI. Reemplaza imágenes, precios, categorías y conecta tu pasarela de pago (Qr Simple, PagosNet, Mercado Pago u otra).</p>
        </div>
      </footer>
    </div>
  );
}
