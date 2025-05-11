# Fullstack Template Next Express

This is a monorepo template for a fullstack application using Next.js for the frontend and Express.js for the backend, managed with Turborepo.

## Project Structure

```
fullstack_template_next_express/
├── apps/
│   ├── api/          # Express.js backend application
│   └── web/          # Next.js frontend application
├── packages/         # Shared packages and utilities
└── turbo.json        # Turborepo configuration
```

## Getting Started

### Prerequisites

- Node.js (v16 or higher)
- pnpm (recommended) or npm

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd fullstack_template_next_express
```

2. Install dependencies:
```bash
npm install
```

### Development

To run the development servers:

```bash
# Run both frontend and backend
npm run dev

# Run only frontend
npm run dev:web

# Run only backend
npm run dev:api
```

### Building

To build all applications:

```bash
npm run build
```

## Available Scripts

- `npm run dev` - Start all applications in development mode
- `npm run build` - Build all applications
- `npm run lint` - Run linting across all applications
- `npm run test` - Run tests across all applications

## Technology Stack

- **Frontend**: Next.js
- **Backend**: Express.js
- **Package Manager**: pnpm
- **Build System**: Turborepo
- **Language**: TypeScript

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contact

gilangfauzan12@gmail.com