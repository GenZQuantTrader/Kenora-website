# Kenora Advisory
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kenora Advisory | Business Consulting</title>
    <style>
        /* --- CSS VARIABLES (Brand Colors based on Logo) --- */
        :root {
            --primary-purple: #3d0e68; /* Deep purple from text */
            --accent-gold: #d9ac2a;    /* Gold from the dot */
            --accent-grey: #7a7a7a;    /* Grey from the dot */
            --light-bg: #f9f9fb;
            --white: #ffffff;
            --text-dark: #333333;
        }

        /* --- GLOBAL STYLES --- */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            color: var(--text-dark);
            background-color: var(--white);
        }

        a {
            text-decoration: none;
            color: inherit;
        }

        ul {
            list-style: none;
        }

        img {
            max-width: 100%;
            height: auto;
        }

        /* --- NAVIGATION --- */
        nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 1rem 5%;
            background-color: var(--white);
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
            position: sticky;
            top: 0;
            z-index: 1000;
        }

        .logo-container {
            height: 50px;
            display: flex;
            align-items: center;
        }

        /* Ensuring the uploaded logo fits nicely */
        .logo-container img {
            max-height: 100%; 
            object-fit: contain;
        }

        .nav-links {
            display: flex;
            gap: 2rem;
        }

        .nav-links a {
            font-weight: 600;
            color: var(--primary-purple);
            transition: color 0.3s;
        }

        .nav-links a:hover {
            color: var(--accent-gold);
        }

        .cta-btn-nav {
            padding: 0.5rem 1.2rem;
            background-color: var(--primary-purple);
            color: var(--white) !important;
            border-radius: 4px;
        }

        .cta-btn-nav:hover {
            background-color: #2a094a;
        }

        /* --- HERO SECTION --- */
        .hero {
            height: 85vh;
            background: linear-gradient(135deg, var(--primary-purple) 0%, #2a094a 100%);
            color: var(--white);
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 0 20px;
        }

        .hero-content h1 {
            font-size: 3.5rem;
            margin-bottom: 1rem;
        }

        .hero-content span {
            color: var(--accent-gold);
        }

        .hero-content p {
            font-size: 1.2rem;
            max-width: 600px;
            margin: 0 auto 2rem auto;
            opacity: 0.9;
        }

        .main-btn {
            padding: 1rem 2.5rem;
            background-color: var(--accent-gold);
            color: var(--primary-purple);
            font-weight: bold;
            font-size: 1.1rem;
            border-radius: 50px;
            border: none;
            cursor: pointer;
            transition: transform 0.2s ease;
        }

        .main-btn:hover {
            transform: translateY(-3px);
            background-color: #c49a22;
        }

        /* --- ABOUT SECTION --- */
        .section {
            padding: 5rem 5%;
        }

        .section-title {
            text-align: center;
            font-size: 2.5rem;
            color: var(--primary-purple);
            margin-bottom: 3rem;
            position: relative;
        }

        .section-title::after {
            content: '';
            display: block;
            width: 60px;
            height: 4px;
            background-color: var(--accent-gold);
            margin: 10px auto 0;
        }

        .about-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 3rem;
            align-items: center;
        }

        .about-text h3 {
            font-size: 1.8rem;
            margin-bottom: 1rem;
            color: var(--accent-grey);
        }

        /* --- SERVICES SECTION --- */
        .bg-light {
            background-color: var(--light-bg);
        }

        .services-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 2rem;
        }

        .service-card {
            background: var(--white);
            padding: 2rem;
            border-radius: 8px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.05);
            transition: transform 0.3s;
            border-top: 5px solid var(--primary-purple);
        }

        .service-card:hover {
            transform: translateY(-5px);
        }

        .service-icon {
            font-size: 2rem;
            margin-bottom: 1rem;
            color: var(--accent-gold);
        }

        .service-card h3 {
            margin-bottom: 1rem;
            color: var(--primary-purple);
        }

        /* --- CONTACT SECTION --- */
        .contact-container {
            max-width: 600px;
            margin: 0 auto;
            text-align: center;
        }

        .contact-info {
            margin-top: 2rem;
            display: flex;
            justify-content: space-around;
            flex-wrap: wrap;
        }

        .contact-item {
            margin: 10px;
        }

        .contact-item h4 {
            color: var(--primary-purple);
        }

        /* --- FOOTER --- */
        footer {
            background-color: var(--primary-purple);
            color: var(--white);
            text-align: center;
            padding: 2rem;
        }

        /* --- MOBILE RESPONSIVENESS --- */
        @media (max-width: 768px) {
            .nav-links {
                display: none; /* Simple hide for mobile in this basic version */
            }
            
            .hero-content h1 {
                font-size: 2.5rem;
            }

            .about-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>

    <nav>
        <div class="logo-container">
            <img src="1000549821.jpg" alt="Kenora Advisory Logo">
        </div>
        <ul class="nav-links">
            <li><a href="#home">Home</a></li>
            <li><a href="#about">About Us</a></li>
            <li><a href="#services">Services</a></li>
            <li><a href="#contact" class="cta-btn-nav">Contact Us</a></li>
        </ul>
    </nav>

    <section id="home" class="hero">
        <div class="hero-content">
            <h1>Strategic Insight. <br><span>Sustainable Growth.</span></h1>
            <p>Kenora Advisory partners with forward-thinking leaders to optimize operations, enhance strategy, and drive measurable business results.</p>
            <button class="main-btn" onclick="document.getElementById('contact').scrollIntoView();">Get a Consultation</button>
        </div>
    </section>

    <section id="about" class="section">
        <div class="section-title">Who We Are</div>
        <div class="about-grid">
            <div class="about-text">
                <h3>Your Trusted Partners in Excellence</h3>
                <p>At Kenora Advisory, we believe that every challenge presents an opportunity. With years of experience across diverse industries, our team brings a wealth of knowledge and a fresh perspective to your business.</p>
                <br>
                <p>We don't just give advice; we walk the path with you, ensuring that our strategies are implemented effectively to create lasting value.</p>
            </div>
            <div class="about-image">
                <div style="background-color: #ddd; height: 300px; border-radius: 8px; display: flex; align-items: center; justify-content: center; color: #666;">
                    [Business Image Placeholder]
                </div>
            </div>
        </div>
    </section>

    <section id="services" class="section bg-light">
        <div class="section-title">Our Services</div>
        <div class="services-grid">
            <div class="service-card">
                <div class="service-icon">●</div>
                <h3>Strategic Planning</h3>
                <p>We help you define your vision and map out the actionable steps required to achieve your long-term goals in a competitive market.</p>
            </div>
            <div class="service-card">
                <div class="service-icon">●</div>
                <h3>Operational Efficiency</h3>
                <p>Streamline your processes, reduce waste, and optimize resources to improve your bottom line and organizational agility.</p>
            </div>
            <div class="service-card">
                <div class="service-icon">●</div>
                <h3>Financial Advisory</h3>
                <p>Expert guidance on capital allocation, investment strategies, and financial restructuring to ensure fiscal health.</p>
            </div>
        </div>
    </section>

    <section id="contact" class="section">
        <div class="section-title">Contact Us</div>
        <div class="contact-container">
            <p>Ready to transform your business? Reach out to Kenora Advisory today.</p>
            
            <div class="contact-info">
                <div class="contact-item">
                    <h4>Email</h4>
                    <p>info@kenoraadvisory.com</p>
                </div>
                <div class="contact-item">
                    <h4>Phone</h4>
                    <p>+1 (555) 123-4567</p>
                </div>
                <div class="contact-item">
                    <h4>Office</h4>
                    <p>123 Business District,<br>City Center, Country</p>
                </div>
            </div>
        </div>
    </section>

    <footer>
        <p>&copy; 2024 Kenora Advisory. All Rights Reserved.</p>
    </footer>

</body>
</html>
