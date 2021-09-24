# swift
// 7_EgorovNikolai


import Foundation

enum shotGunError: Error {
    case noBullets(bulletsneed: Int)
    case noFreeGun
    case noGun
}

struct Guns {
    var bullets: Int
    var clip: Int 
}

class  tirGuns {
    
    var inventory = [
        "USP-S": Guns(bullets: 12, clip: 4),
        "Glock-18": Guns(bullets: 20, clip: 10),
        "Deagle": Guns(bullets: 7, clip: 5),
        "AK-47" : Guns(bullets: 30, clip: 3),
        "M4A1" : Guns(bullets: 25, clip: 2)
    ]
    
    var gunsBullets = 0
    
    func errors(gunName name: String) throws {
        guard let guns = inventory[name] else {
            throw shotGunError.noGun
        }
        guard guns.clip > 0 else {
            throw shotGunError.noGun
        }
        
        guard guns.bullets <= gunsBullets else {
            throw shotGunError.noBullets(bulletsneed: guns.bullets - gunsBullets)
        }
        
        gunsBullets -= guns.bullets
        
        var someGuns = guns
        someGuns.bullets -= 1
        inventory[name] = someGuns
        print("Взял оружие \(name)")
    }
    
}

let favoriteGun = [
    "Igor": "Glock-18",
    "Andrei": "AK-47",
    "Victor": "Deagle"
]

func voteGuntier(player: String, Guns: tirGuns) throws {
    let playerName = favoriteGun[player] ?? "Deagle"
    try Guns.errors(gunName: playerName)
}

var player = tirGuns()
player.gunsBullets = 20

do {
    try voteGuntier(player: "Andrei", Guns: player)
} catch shotGunError.noFreeGun {
    print("Нет свободного оружия")
} catch shotGunError.noGun {
    print("В тире нет такого оружия")
} catch shotGunError.noBullets(let bulletsNeed) {
    print("Недостаточно патронов, нужно еще \(bulletsNeed)")
}
