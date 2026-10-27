# 🛒 Feira Livre Digital — CEASA São Luís

Aplicação web completa e responsiva para conectar consumidores aos feirantes e produtores locais da CEASA em São Luís - MA, integrada em tempo real com o **Supabase**.

## 🚀 Tecnologias Utilizadas
- **Frontend:** HTML5, Tailwind CSS, FontAwesome, Google Fonts.
- **Backend/Banco de Dados:** Supabase (`lfdqyeetxmgjgzzlzpgo`).
- **Navegação & Frete:** OpenStreetMap / OSRM API.

## 🗄️ Estrutura do Banco de Dados (Supabase SQL)

Execute o script abaixo no **SQL Editor** do Supabase para criar e configurar as tabelas com permissões públicas de leitura e escrita:

```sql
-- 1. Tabela de Produtos
create table if not exists public.products (
  id bigint primary key generated always as identity,
  name text not null,
  category text not null,
  price numeric not null,
  unit text not null,
  image text,
  rating numeric default 5.0,
  banca text not null,
  created_at timestamp with time zone default timezone('utc'::text, now())
);

-- 2. Tabela de Pedidos
create table if not exists public.orders (
  id text primary key,
  date text not null,
  customer_name text,
  customer_phone text,
  customer_address text,
  items jsonb not null,
  subtotal numeric,
  service_fee numeric,
  delivery_fee numeric,
  total numeric not null,
  payment_method text,
  change_for text,
  status text default 'Em Preparo',
  delivery text,
  created_at timestamp with time zone default timezone('utc'::text, now())
);

-- 3. Tabela de Feirantes
create table if not exists public.feirantes (
  id bigint primary key generated always as identity,
  name text not null,
  pin text not null,
  whatsapp text not null,
  status text default 'approved',
  created_at timestamp with time zone default timezone('utc'::text, now())
);

-- Habilitar Row Level Security (RLS) e liberar leitura/escrita
alter table public.products enable row level security;
alter table public.orders enable row level security;
alter table public.feirantes enable row level security;

drop policy if exists "Acesso público total a produtos" on public.products;
drop policy if exists "Acesso público total a pedidos" on public.orders;
drop policy if exists "Acesso público total a feirantes" on public.feirantes;

create policy "Acesso público total a produtos" on public.products for all using (true) with check (true);
create policy "Acesso público total a pedidos" on public.orders for all using (true) with check (true);
create policy "Acesso público total a feirantes" on public.feirantes for all using (true) with check (true);
```

## 💻 Como Publicar no GitHub Pages
1. Crie um novo repositório no GitHub.
2. Faça upload dos arquivos `index.html`, `README.md`, `.gitignore` e `LICENSE`.
3. Acesse **Settings > Pages** no seu repositório.
4. Selecione a branch `main` e a pasta `/ (root)` e salve.
