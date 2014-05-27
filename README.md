SecretSharing
=============

Tools for sharing secrets (like Bitcoin private keys), using shamir's secret sharing scheme

## Sample Usage

### Hex Secrets

#### Splitting into shares

    >>> from secretsharing import SecretSharer
    >>> secret = "c4bbcb1fbec99d65bf59d85c8cb62ee2db963f0fe106f483d9afa73bd4e39a8a"
    >>> sharer = SecretSharer()
    >>> shares = sharer.split_secret(secret, 2, 3)
    ['1-58cbd30524507e7a198bdfeb69c8d87fd7d2c10e8d5408851404f7d258cbcea7', '2-ecdbdaea89d75f8e73bde77a46db821cd40f430d39a11c864e5a4868dcb403ed', '3-80ebe2cfef5e40a2cdefef0923ee2bb9d04bc50be5ee308788af98ff609c380a']

#### Recovering from shares

    >>> sharer.recover_secret(shares[0:3])
    'c4bbcb1fbec99d65bf59d85c8cb62ee2db963f0fe106f483d9afa73bd4e39a8a'

### Plaintext Secrets

#### Splitting into shares

    >>> from secretsharing import PlaintextToHexSecretSharer
    >>> secret = "correct horse battery staple"
    >>> sharer = PlaintextToHexSecretSharer()
    >>> shares = sharer.split_secret(secret, 2, 3)
    ['1-7da6b11af146449675780434f6589230a3435d9ab59910354205996f508b8d0d', '2-fb4d6235e28c892cea70367c15ec3cbfed4cf4a417bd01e9812980f3ac97ddc8', '3-78f41350d3d2cdc35f6868c3357fe74f37568bad79e0f39dc04d687808a42d5a']

#### Recovering from shares

    >>> sharer.recover_secret(shares[0:2])
    'correct horse battery staple'

### Bitcoin Private Keys

#### Splitting into reliably transcribable shares

    >>> from secretsharing import BitcoinSecretSharer
    >>> secret = "5KJvsngHeMpm884wtkJNzQGaCErckhHJBGFsvd3VyK5qMZXj3hS"
    >>> sharer = BitcoinSecretSharer()
    >>> shares = sharer.split_secret(secret, 2, 3)
    ['b-aweuzkm9jmfgd7x4k595bzcm3er3epf4dprfwzpprqa3exbuocs9byn4owfuqbo', 'n-btetgqqu8doacarsbyfdzpyycyj6gfdeaaxrpfx33pdjk4ou1d5owjdmdi1iegm9', 'd-njh33f14q7smucmh8iq8uaewc8mzub3mzptrwsegfiz3hc1fozkkjtguc4trh6sq']

#### Recovering from shares

    >>> sharer.recover_secret(shares[0:2])
    '5KJvsngHeMpm884wtkJNzQGaCErckhHJBGFsvd3VyK5qMZXj3hS'    

### Raw integers

#### Splitting into shares

    >>> from secretsharing import secret_int_to_points, points_to_secret_int
    >>> secret = 88985120633792790105905686761572077713049967498756747774697023364147812997770L
    >>> shares = secret_int_to_points(secret, 2, 3)
    [(1, 108834987130598118322155382953070549297972563210322923466700361825476188819879L), (2, 12892764390087251114834094135881113029625174256248535119246116278891435001755L), (3, 32742630886892579331083790327379584614547769967814710811249454740219810823864L)]

#### Recovering from shares

    >>> points_to_secret_int(shares[0:2])
    88985120633792790105905686761572077713049967498756747774697023364147812997770L
