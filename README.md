# Kubernetes Multi-Tenant Resource Guard

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![YAML](https://img.shields.io/badge/YAML-CB171E?style=for-the-badge&logo=yaml&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

> Kompleksowy system izolacji i optymalizacji zasobów w środowisku wielotenantowym Kubernetes.
>
> Implementacja wykorzystuje mechanizmy **Quota**, **LimitRange** oraz **NetworkPolicies** w celu zapewnienia pełnej stabilności i bezpieczeństwa lokatorów.
>
> 📄 Projekt udostępniany na licencji **MIT**. Szczegóły w pliku [LICENSE.md](./LICENSE.md).

## 📋 Spis treści

- [O projekcie](#-o-projekcie)
- [Kluczowe funkcjonalności](#-kluczowe-funkcjonalności)
- [Struktura projektu](#-struktura-projektu)
- [Architektura](#-architektura)
- [Instalacja i użycie](#️-instalacja-i-użycie)

## 🚀 O projekcie

Projekt rozwiązuje problem bezpiecznego współdzielenia zasobów klastra przez wiele niezależnych zespołów (Ecommerce oraz Marketing). Implementacja gwarantuje stabilność usług krytycznych oraz pełną izolację sieciową między lokatorami, co zostało potwierdzone testami obciążeniowymi (CPU Throttling, OOMKilled).

## ✨ Kluczowe funkcjonalności

- **Izolacja zasobowa:** Twarde limity budżetu CPU/RAM za pomocą `ResourceQuota`.
- **Domyślne limity:** Automatyczne narzucanie limitów kontenerom przez `LimitRange`.
- **Priorytetyzacja (QoS):** Wykorzystanie `PriorityClass` dla usług krytycznych.
- **Bezpieczeństwo sieciowe:** Blokowanie ruchu między namespace'ami przez `NetworkPolicies`.
- **Dynamiczne skalowanie:** `HPA` dostosowujące liczbę replik do obciążenia procesora.

## 📁 Struktura projektu

```bash
k8s-multi-tenant-resource-guard/
├── LICENSE.md
├── README.md
├── .gitignore
└── manifests/
    ├── cluster-wide/
    │   └── priorities.yaml
    ├── ecommerce-prod/
    │   ├── ecommerce-app.yaml
    │   ├── hpa-shop.yaml
    │   └── network-isolation.yaml
    └── marketing-dev/
        ├── default-limits.yaml
        ├── marketing-app.yaml
        └── marketing-quota.yaml
```

## 🧩 Architektura

System opiera się na logicznej i fizycznej separacji:

- `ecommerce-prod`: Wysoki priorytet, dedykowane węzły wydajnościowe (`NodeAffinity`).
- `marketing-dev`: Niższy priorytet, restrykcyjne kwoty zasobowe dla procesów tła.

## 🛠️ Instalacja i użycie

Aby wdrożyć konfigurację w klastrze, wykonaj:

```bash
# Sklonuj repozytorium i przejdź do katalogu projektu
git clone https://github.com/mavethee/k8s-multi-tenant-guard.git && cd k8s-multi-tenant-guard

# Wdróż priorytety i konfigurację klastra
kubectl apply -f manifests/cluster-wide/

# Wdróż konfigurację dla Ecommerce
kubectl apply -f manifests/ecommerce-prod/

# Wdróż konfigurację dla Marketingu
kubectl apply -f manifests/marketing-dev/
```

> Stworzono z myślą o nauce i rozwoju 😄  
> Autor: **Marcin Mitura**  
> Data: `2026-01-22`
