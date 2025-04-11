# Cluster - Claim Community Name

A Next.js application that allows users to authenticate with [AppKit](https://reown.com/appkit) and claim their community name.

## Application Flow

1. **Authentication**
   - Users connect their wallet using AppKit's authentication system
   - Supports multiple wallet types and traditional authentication methods

2. **Wallet Connection**
   - After successful authentication, the app displays the connected wallet address
   - Uses wagmi's `useAccount` hook to access `isConnected` and `address` to manage wallet state

3. **Community Name Management**
   - Once authenticated or connected, users can:
     - View their existing community name (`GET /v1/names/address/{walletAddress}`)
     - Claim a new community name if they don't have one

## API Integration

### Pre-req

1. **Register a Community Cluster**
    - [Register via GUI](https://clusters.xyz/register)
    - [Register via API](https://docs.clusters.xyz/getting-started/api/v1/registration)

2. **Authenticate Cluster Wallet**
    - Only the wallet that owns the Community Cluster can add new wallets as members.
    - Get authentication key to use later when adding wallets.
    - [Authentication API spec](https://docs.clusters.xyz/getting-started/api/v1/authentication)

### Clusters API Endpoints Used

1. **Fetch Community Name**
- [Get Name API spec](https://docs.clusters.xyz/getting-started/api/v1/address-cluster-name#get-name)
```bash
GET https://api.clusters.xyz/v1/names/address/${walletAddress}

# Returns: { walletName: string }
```

2. **Claim Community Name**
- [Add Wallets API spec](https://docs.clusters.xyz/getting-started/api/v1/clusters#add-wallets)
- The `NEXT_PUBLIC_CLUSTER_COMMUNITY_AUTH_KEY` variable is the authentication key generated by the verified wallet (owner of the Community Cluster).

```bash
# API route: /api/cluster

POST https://api.clusters.xyz/v1/clusters/wallets
Headers: {
  X-API-Key: `${process.env.NEXT_PUBLIC_CLUSTERS_API_KEY}`,
  Authorization: `Bearer ${process.env.NEXT_PUBLIC_CLUSTER_COMMUNITY_AUTH_KEY}`
}
Body: { walletAddress: string, name: string }
```

## Environment Variables

Required environment variables:
```bash
NEXT_PUBLIC_CLUSTERS_API_KEY=  # Cluster API Key
NEXT_PUBLIC_REOWN_PROJECT_ID=  # Reown API key
NEXT_PUBLIC_CLUSTER_COMMUNITY_AUTH_KEY=  # Community Clusters Wallet authentication key
```

## Getting Started

1. Clone the repository
2. Install dependencies:
```bash
npm install
```

3. Create a `.env.local` file with the required environment variables

4. Run the development server:
```bash
npm run dev
```

5. Open [http://localhost:3000](http://localhost:3000) in your browser

## Tech Stack

- Next.js 14 (App Router)
- Reown AppKit SDK for auth and wallet provider
- Clusters API for name management
- TypeScript
- Tailwind CSS

## Project Structure

```text
src/
├── app/
│   ├── page.tsx           # Main page with AppKit authentication
│   └── api/
│       └── cluster/       # API route for Clusters integration
│           └── route.ts
├── components/
│   └── ClusterDisplay.tsx # Manages community name
└── config/
│    └── index.tsx       # Network config and Wagmi adapter setup
└── context/
    └── index.tsx        # AppKit and Wagmi provider configuration
```